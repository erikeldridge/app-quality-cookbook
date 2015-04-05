Service development

In this section, we'll
* create a feature switch service using the code you wrote in the [maintainability section](maintainability.md)
* gain experience with build-, launch-, and run-time configuration
* use configuration and version control for damage control
* touch on versioning endpoints for backwards compatability
* use tracing and analytics to monitory app usage
* use dynamic routing to facilitate testing in different environments
* gain experience with fractional, staged, and automated deployment
* expand on our dependency injection (DI) experience using Dagger 2
* learn about redline testing

## Create a Java web service using Jersey

[Jersey](https://jersey.java.net/) is a framework we can use for creating HTTP APIs.

[HTTP](http://www.w3.org/Protocols/rfc2616/rfc2616.html) is a protocol used for transferring content, like a web page, over a network. In the case of web pages, content is structured using [HTML](http://www.w3.org/TR/2014/REC-html5-20141028/). In our case, we'll structure data using [JSON](http://json.org/).

Jersey implements a pattern called [REST](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm), which organizes APIs using HTTP and URL conventions. For example, we can use the HTTP verb _GET_ to get feature switch configuration from a URL like _http://example.com/feature_switch_config_.

Run the Maven archetype command from the _Creating a Web Application that can be deployed on Heroku_ section of [Jersey's getting started documentation](https://jersey.java.net/documentation/latest/getting-started.html):

    $ mvn archetype:generate -DarchetypeArtifactId=jersey-heroku-webapp \
    -DarchetypeGroupId=org.glassfish.jersey.archetypes -DinteractiveMode=false \
    -DgroupId=com.example -DartifactId=simple-heroku-webapp -Dpackage=com.example \
    -DarchetypeVersion=2.17

[Heroku](https://www.heroku.com/), btw, is a fantastic hosting platform. We'll explore it later in this section.

This will create a directory called "simple-heroku-webapp".

Create a new IntelliJ project by importing _simple-heroku-webapp/pom.xml_ and accepting all the defaults, as we've done before with our other Maven projects.

Open the _Main_ class (IntelliJ tip: type [ctrl] + N to search by class name) and observe we're setting the port to "8080" by default and the base path to "/":

    ...
    String webPort = System.getenv("PORT");
    if (webPort == null || webPort.isEmpty()) {
        webPort = "8080";
    }

    final Server server = new Server(Integer.valueOf(webPort));
    final WebAppContext root = new WebAppContext();

    root.setContextPath("/");
    ...

This configures the service to respond to HTTP requests for URLs beginning with _http://localhost:8080/_

Open the _MyResource_ class and observe we're defining code to return "Hello, Heroku!" in response to an HTTP GET request for _myresource_:

    ...
@Path("myresource")
public class MyResource {
    ...
    @GET
    @Produces(MediaType.TEXT_PLAIN)
    public String getIt() {
        return "Hello, Heroku!";
    ...

We should be able to call _http://localhost:8080/myresource_ when the server is running and get "Hello, Heroku!" back.

In your terminal, change into the _simple-heroku-webapp_ directory:

    $ cd simple-heroku-webapp

Once we start the server, it will "take over" the terminal and we want to work while the server is running, so open a new terminal tab.

Compile and run your server:

    $ mvn compile jetty:run

The `jetty:run` command starts a web server called [Jetty](http://www.eclipse.org/jetty/). Jersey works with a number of web servers. This project's _pom.xml_ file specifies Jetty, which is advantageous because Jetty is also used by projects like [Google AppEngine](http://www.infoq.com/news/2009/08/google-chose-jetty) and [Drop Wizard](http://www.dropwizard.io), so our experience will be portable.

Change to your other tab and make a request to the server using the [curl](http://curl.haxx.se/) tool:

    $ curl http://localhost:8080/myresource
    Hello, Heroku!

Congratulations! You've just defined a RESTful API endpoint.

Take a look at the test defined by _MyResourceTest_ and observe how it asserts the value of the [response](http://www.w3.org/Protocols/rfc2616/rfc2616-sec6.html) is "Hello, Heroku!".

Run your tests to verify the generated tests work:

    $ mvn test

All right. We've got a working version, so let's commit it. Initialize a new repository:

    $ git init

See what's in it:

    $ git status
    On branch master

    Initial commit

    Untracked files:
      (use "git add <file>..." to include in what will be committed)

        .idea/
        Procfile
        pom.xml
        simple-heroku-webapp.iml
        src/
        system.properties
        target/

Create a .gitignore file to ignore IntelliJ's ".idea" directory and the "target" directory, which contains our compiled code:

    .idea
    *.iml
    target

Now add and commit everything:

    $ git add .
    $ git commit -m "Generate Jersey app"

## Use self-descriptive names

Rename your service to something more self-descriptive:

    $ cd ..
    $ mv simple-heroku-webapp feature-switch-service
    $ cd feature-switch-service

Edit _pom.xml_ to match your directory name:

    <groupId>com.example</groupId>
    <artifactId>feature-switch-service</artifactId>
    <packaging>jar</packaging>
    <version>1.0-SNAPSHOT</version>
    <name>feature-switch-service</name>

Modify the resource path to be more self-descriptive:

    ...
    @Path("feature_switch_config")
    ...

Use IntelliJ's refactoring tools to rename the request handler and test classes to FeatureSwitchConfig and FeatureSwitchConfigTest, respectively. Rename the request handler function from _getIt_ to _get_.

Your diff will look like this:

    $ git diff
    diff --git a/Procfile b/Procfile
    index 3da6f26..8314a3f 100644
    --- a/Procfile
    +++ b/Procfile
    @@ -1 +1 @@
    -web: java -cp target/classes:target/dependency/* com.example.heroku.Main
    \ No newline at end of file
    +web: java -cp target/classes:target/dependency/* com.example.featureswitchservice.Main
    \ No newline at end of file
    diff --git a/pom.xml b/pom.xml
    index dca6042..1af7682 100644
    --- a/pom.xml
    +++ b/pom.xml
    @@ -4,10 +4,10 @@
         <modelVersion>4.0.0</modelVersion>
     
         <groupId>com.example</groupId>
    -    <artifactId>simple-heroku-webapp</artifactId>
    +    <artifactId>feature-switch-service</artifactId>
         <packaging>war</packaging>
         <version>1.0-SNAPSHOT</version>
    -    <name>simple-heroku-webapp</name>
    +    <name>feature-switch-service</name>
     
         <dependencyManagement>
             <dependencies>
    @@ -61,7 +61,7 @@
         </dependencies>
     
         <build>
    -        <finalName>simple-heroku-webapp</finalName>
    +        <finalName>feature-switch-service</finalName>
             <plugins>
                 <plugin>
                     <groupId>org.apache.maven.plugins</groupId>
    diff --git a/src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java b/src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java
    index 7d05472..8392f6c 100644
    --- a/src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java
    +++ b/src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java
    @@ -19,7 +19,7 @@ public class FeatureSwitchConfig {
          */
         @GET
         @Produces(MediaType.TEXT_PLAIN)
    -    public String getIt() {
    +    public String get() {
             return "Hello, Heroku!";
         }
     }
    diff --git a/src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java b/src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java
    index 51afde3..849230e 100644
    --- a/src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java
    +++ b/src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java
    @@ -19,8 +19,8 @@ public class FeatureSwitchConfigTest extends JerseyTest {
          * Test to see that the message "Got it!" is sent in the response.
          */
         @Test
    -    public void testGetIt() {
    -        final String responseMsg = target().path("myresource").request().get(String.class);
    +    public void testGet() {
    +        final String responseMsg = target().path("feature_switch_config").request().get(String.class);
     
             assertEquals("Hello, Heroku!", responseMsg);
         }

In the terminal, type the _ctrl + C_ to stop the server. Recompile and run your server, and functionally test via curl:

    $ curl http://localhost:8080/feature_switch_config
    Hello, Heroku!

See what's changed:

    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

      renamed:    src/main/java/com/example/heroku/MyResource.java -> src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java
      renamed:    src/main/java/com/example/heroku/Main.java -> src/main/java/com/example/featureswitchservice/Main.java
      renamed:    src/test/java/com/example/heroku/MyResourceTest.java -> src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java

    Changes not staged for commit:
      (use "git add <file>..." to update what will be committed)
      (use "git checkout -- <file>..." to discard changes in working directory)

      modified:   Procfile
      modified:   pom.xml
      modified:   src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java
      modified:   src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java

Add your changes (using the _--all_ flag to include the deleted files):

    $ git add --all .

Re-run `git status` to verify git has interpreted file deletion and creation as a rename:

    $ git status
    On branch master
    Changes to be committed:
      (use "git reset HEAD <file>..." to unstage)

      modified:   Procfile
      modified:   pom.xml
      renamed:    src/main/java/com/example/heroku/MyResource.java -> src/main/java/com/example/featureswitchservice/FeatureSwitchConfig.java
      renamed:    src/main/java/com/example/heroku/Main.java -> src/main/java/com/example/featureswitchservice/Main.java
      renamed:    src/test/java/com/example/heroku/MyResourceTest.java -> src/test/java/com/example/featureswitchservice/FeatureSwitchConfigTest.java

Commit:

    $ git commit -m "Reformat project to be more self-descriptive"

## Add your feature switch code to this project

We have a couple options for getting the code we created in the maintainability section into our project:

1. Copy/paste it over
1. Import it via Maven

The first option is easier, but breaks encapsulation. The second option requires some set up, but will result in a more maintainable project. I'll cover them both.

If you copy/paste it over, you should end up with a set of files like:

    $ ls src/main/java/com/example/
    Input.java FeatureSelector.java FeatureSwitchConfig.java  Main.java
    $ ls src/test/java/com/example/
    FeatureSelectorTest.java FeatureSwitchConfigTest.java
    $ ls src/test/resources/
    features.yml

Test to make sure everything works:

    $ mvn test

Commit your changes.

If you import the project you'll need to package your code, declare a dependency, and import that dependency.

Package your feature selector as a jar file by changing into that project's directory and running `mvn package`:

    $ cd ../feature-selector
    $ mvn package
    [INFO] Scanning for projects...
    ...
    [INFO] ------------------------------------------------------------------------
    [INFO] Building feature-selector 1.0-SNAPSHOT
    [INFO] ------------------------------------------------------------------------
    ...
    [INFO] 
    [INFO] --- maven-jar-plugin:2.2:jar (default-jar) @ feature-selector ---
    [INFO] Building jar: /home/vagrant/feature-selector/target/feature-selector-1.0-SNAPSHOT.jar
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 2.449s
    [INFO] Finished at: Fri Apr 03 06:11:15 UTC 2015
    [INFO] Final Memory: 12M/29M
    [INFO] ------------------------------------------------------------------------
    $ ls target
    ... feature-selector-1.0-SNAPSHOT.jar ...

Change back into _feature-switch-service_ and declare your dependency using a relative path in _pom.xml_:

    <dependencies>
        ...
        <dependency>
            <version>1.0-SNAPSHOT</version>
            <groupId>com.example</groupId>
            <artifactId>feature-selector</artifactId>
            <scope>system</scope>
            <systemPath>${project.basedir}/../feature-selector/target/feature-selector-1.0-SNAPSHOT.jar</systemPath>
        </dependency>
        ...
    </dependencies>

Compile your project to pull in your dependency:

    $ mvn compile

Observe the warning regarding inaccessibility of this locally referenced jar:

...
[WARNING] 'dependencies.dependency.systemPath' for com.example:feature-selector:jar should not point at files within the project directory, ${project.basedir}/../feature-selector/target/feature-selector-1.0-SNAPSHOT.jar will be unresolvable by dependent projects @ line 40, column 25
...

This is fine since we're the only ones working on this project, but when working with teams, we can use tools like [Nexus](http://www.sonatype.org/nexus/) or [Bintray](https://bintray.com/) to distribute our artifacts.

Edit _FeatureSwitchConfig_ to import _FeatureSelector_:

    ...
    import com.example.FeatureSelector;
    ...

Observe IntelliJ can resolve this reference and _com.example.FeatureSelector_ appears in the list of "External Libraries" in IntelliJ's project structure.

Commit your _pom.xml_ changes ("Add feature-selector dependency").

## Integrate feature selector

We now have a web server and a class for generating feature switch config. Let's tie them together.

Define a _features.yml_ file in _src/main/resources_, like we did in the _feature-selector_ project.

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

If you need to modify your feature selector code during your integration, you can open both projects in separate [IntelliJ windows](https://www.jetbrains.com/idea/help/opening-multiple-projects.html#d1286669e148). Run `mvn package` after any feature selector changes to repackage the _jar_ file imported into the feature switch service project.

Update your integration tests once your functional testing with curl works.

    ...
    @Test
    public void testGet() throws IOException {
        final String responseMsg = target()
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
        assertEquals(expected.toString(), responseMsg);
    }
    ...

Commit your changes, especially since the upcoming JSON integration is complicated.

Next, we want to return the config as JSON so external callers can easily parse it. We can use a tool called Jackson to convert our Java objects to JSON.

To do that, we'll need to configure our project, Jersey, and our app logic to use Jackson for JSON parsing.

Add a dependency on Jersey's Jackson artifact in your _pom.xml_:

    ...
    <dependency>
        <groupId>org.glassfish.jersey.media</groupId>
        <artifactId>jersey-media-json-jackson</artifactId>
        <version>${jersey.version}</version>
    </dependency>
    ...

Define a new _ResourceConfig_ class to register Jackson (under the hood, Jersey is using dependency injection to inject Jackson):

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

Recompile, test functionally via curl, run your integration test, and commit when everything's working.

## Test using your VM

We can treat our VM as a remote server by configuring _port forwarding_ and then calling our service from our local maching.

Edit your Vagrantfile to uncomment the _forward_port_ line:

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    config.vm.network "forwarded_port", guest: 8080, host: 80

Reload your VM to apply the configuration change:

    $ vagrant reload

Observe port forwarding details logged by the VM as it starts up:

    $ vagrant reload
    ...
    ==> default: Forwarding ports...
        default: 8080 => 8080 (adapter 1)
        default: 22 => 2222 (adapter 1)
    ...

After the VM restarts, _ssh_ in and restart your server:

    $ vagrant ssh
    $ cd feature-switch-service
    $ mvn jetty:run

Load [http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3](http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3) in a browser on your local machine. Observe your server running in the VM handles the request.

## Deploy the app to Heroku

## Observe deploy log

## Observe we need to deploy to change config

## Modify the app to abstract config as a dependency

Inject the config using Dagger 2

Load config dynamically (from Github API)

Observe we also get a commit log

Read [Knightmare: A DevOps Cautionary Tale](http://dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/)

Use launch and dynamic (fractional deploy) config to switch sources

Observe any client ingesting this config will need to handle the off state

Define staging version of your config service

Run your test script against your VM, then staging, then prod (staged deploy)

Bonus: define canary instance

Split your github config loading into a config service (this is a bit contrived, but it will help demonstrate a couple points)

Define a trace header

Log and relay this trace header

Define a dynamic routing header

Route requests from your front-facing server to your config service using your dynamic routing header

Verify requests using your trace id

Integrate with logly and query for your trace id

Define a script that builds, tests, deploys to staging, tests, deploys to prod, tests (automated deploy)

Rollback your last deploy

Integrate monitoring using New Relic

Observe requests in New Relic

Define redline tests

Define alerts in New Relic

Observe redline visualized

Observe alerts
