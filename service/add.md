# Add your feature switch code to this project

We have a couple options for getting the code we created earlier into our project:

1. Import it via Maven
1. Copy/paste it over

The first preserves encapsulation, which would help with maintainability as our project grows. We can package our code as an artifact and use tools like [Bintray](https://bintray.com/) or [Nexus](http://www.sonatype.org/nexus/) to distribute it.

However, since we're building a configuration service, we're the only customers of this code, and the focus of this section is service creation, not artifact distribution, copying the code over seems acceptable.

If you copy/paste it over, you should end up with a diff like this:

    $ git diff --name-status
    M       pom.xml
    A       src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java
    A       src/main/resources/features.yml
    A       src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java
    A       src/test/resources/features.yml

We've modified _pom.xml_ to include our feature selector dependencies and added a few new files.

Test to make sure everything works:

    $ mvn test

Commit your changes.

## Integrate feature selector

We now have a web server and code for generating feature switch config. Let's tie them together.

Edit _FeatureSwitchConfig_ to accept args from the request URL and return feature switch configuration:

    ...
    @Path("feature_switch_config")
    public class FeatureSwitchConfig {
        @QueryParam("id") String id;
        @QueryParam("os") String os;
        @QueryParam("version") String version;

        @GET
        @Produces(MediaType.TEXT_PLAIN)
        public String get() throws FileNotFoundException, YamlException {
          Input input = FeatureSelector.requireInput(new String[]{id, os, version});
          Map<String, Map<String, String>> features = FeatureSelector.createFeatureMap();
          FeatureSelector selector = new FeatureSelector(features);
          Map<String, Boolean> selected = selector.select(input);
          return selected.toString();
        }
    }
    ...

Recompile and run your server:

    $ mvn compile jetty:run

Call your endpoint:

    $ curl "http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3"
    {feature_c=true, feature_b=false, feature_a=true}

The next step is to format our response as JSON to external callers can easily parse it.

However, the steps to integrate JSON are complicated and it would be nice to save our state. We only want to commit working code, though, so create a temp branch to encapsulate the changes.

    $ git branch integration_json
    $ git checkout integration_json

We can use a tool called Jackson to convert our Java objects to JSON. To do that, we'll need to make changes to our project, Jersey, and app logic.

Add a dependency on Jersey's Jackson artifact in your _pom.xml_:

    ...
    <dependency>
        <groupId>org.glassfish.jersey.media</groupId>
        <artifactId>jersey-media-json-jackson</artifactId>
        <version>${jersey.version}</version>
    </dependency>
    ...

Define a new _ResourceConfig_ class to register Jackson:

    package com.example.featureswitchservice;

    import org.glassfish.jersey.jackson.JacksonFeature;
    import org.glassfish.jersey.server.ResourceConfig;

    public class App extends ResourceConfig {
      public App() {
        register(JacksonFeature.class);
      }
    }

Modify _src/main/webapp/WEB-INF/web.xml_ so Jersey is aware of Jackson:

    ...
    <init-param>
        <param-name>javax.ws.rs.Application</param-name>
        <param-value>com.example.featureswitchservice.App</param-value>
    </init-param>
    <init-param>
        <param-name>jersey.config.server.provider.packages</param-name>
        <param-value>com.example.featureswitchservice;org.codehaus.jackson.jaxrs</param-value>
    </init-param>
    ...

Modify your request handler to return a map of values as _application/json_:

    ...
    @GET
    @Produces(MediaType.APPLICATION_JSON)
    public Map<String, Boolean> get() throws FileNotFoundException, YamlException {
      ...
      Map<String, Boolean> selected = selector.select(input);
      return selected;
    }
    ...

Modify your test to parse the response using [Jackson's ObjectMapper](http://wiki.fasterxml.com/JacksonInFiveMinutes#Examples):

    @Test
    public void testGet() throws IOException {
        final String json = target()
            .path("feature_switch_config")
            .queryParam("id", "123")
            .queryParam("os", "android")
            .queryParam("version", "2.3")
            .request()
            .get(String.class);
        HashMap<String, Boolean> expected = new HashMap<>();
        expected.put("feature_a", true);
        expected.put("feature_b", false);
        expected.put("feature_c", true);
        ObjectMapper mapper = new ObjectMapper();
        Map<String, Boolean> actual = mapper.readValue(json, HashMap.class);
        assertEquals(expected, actual);
    }

Run your integration tests to verify they're still working.

Recompile, run your server, and call with _curl_:

    $ curl -v "http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3"
    ...
    {"feature_c":true,"feature_b":false,"feature_a":true}

Observe the _Content-Type_ response header is now "application/json":
    ...
    < Content-Type: application/json
    ...
    
Commit your changes and merge your branch back into your development branch:

    $ git commit -m "Integrat feature selector"
    $ git checkout dev
    $ git merge integrate_json
