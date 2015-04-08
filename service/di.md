# Modify the app to abstract config as a dependency

Observe we need to deploy to change config

To accomodate loading config from a dynamic source, abstract your configuration as a dependency.

This will also help us achieve a [hermetic server](http://googletesting.blogspot.com/2012/10/hermetic-servers.html).

We'll use Guice to manage our dependency injection for a few reasons:
* it's very mature, so we can easily find documentation
* we'll be using Dagger DI, which is an iteration on Guice for Android
* in the near future, we'll use Dagger 2 for client and server DI

Guice defines a few basic concepts:
* Modules define dependency providers
* Providers define a function that returns an instance of the dependency we want
* Injectors inject the dependencies defined by modules into classes

In terms of Jersey, we'll configure it to inject our configuration into our request handler. We can then inject different sources of configuration in our development, production, and test environments. For example, we could load from a static variable in development, a mock object in tests, and a remote service in production.

First, define our module:

    package com.example.featureswitchservice;

    import com.esotericsoftware.yamlbeans.YamlException;
    import com.fasterxml.jackson.databind.ObjectMapper;
    import com.fasterxml.jackson.jaxrs.json.JacksonJsonProvider;
    import com.google.inject.Provides;
    import com.google.inject.Singleton;
    import com.sun.jersey.guice.JerseyServletModule;
    import com.sun.jersey.guice.spi.container.servlet.GuiceContainer;

    import javax.inject.Named;
    import java.io.FileNotFoundException;
    import java.util.Map;

    public class ProductionModule extends JerseyServletModule {

      @Provides
      @Singleton
      ObjectMapper objectMapper() {
        return new ObjectMapper();
      }

      @Provides
      @Singleton
      JacksonJsonProvider jacksonJsonProvider(ObjectMapper mapper) {
        return new JacksonJsonProvider(mapper);
      }

      @Provides
      @Named("config")
      Map<String, Map<String, String>> get() throws FileNotFoundException, YamlException {
        return FeatureSelector.createFeatureMap();
      }

      @Override
      protected void configureServlets() {
        bind(ConfigResource.class);
        serve("/*").with(GuiceContainer.class);
      }
    }

Observe we're still calling `createFeatureMap` to load config from our resource file. We'll update that after migrating to Guice, so we can run our existing tests to verify Guice is working as expected.

Functions with the `@Provides` annotation will be called by Guice whenever we try to inject an instance of the class returned by the function.

For example, Guice will call `jacksonJsonProvider` wherever we inject a `JacksonJsonProvider` instance.

The `@Named` annotation enables us to differentiate instances of a common class, like `Map` or `String`, by name.

The `@Singleton` annotation tells Guice to only instantiate this object once. A big advantage of DI is [singleton](http://sourcemaking.com/design_patterns/singleton) management. Singletons can be very difficult to test, especially if they are implemented using global variables. With DI, we don't need to maintain the conditional instantiation code, and we can easily override it in our test environment.

Next, modify the `Main` class (generated earlier by the Jersey Heroku archetype) to inject these dependencies:

    package com.example.featureswitchservice;

    import com.google.inject.Guice;
    import com.google.inject.Injector;
    import com.google.inject.servlet.GuiceFilter;
    import com.google.inject.servlet.GuiceServletContextListener;
    import org.eclipse.jetty.server.Server;
    import org.eclipse.jetty.servlet.DefaultServlet;
    import org.eclipse.jetty.servlet.ServletContextHandler;
    import org.eclipse.jetty.webapp.WebAppContext;

    public class Main {
      static final int DEFAULT_PORT = 8080;

      public static int normalizePort(String port) {
        if (port == null || port.isEmpty()) {
          return DEFAULT_PORT;
        } else {
          return Integer.valueOf(port);
        }
      }

      public static Server createServer(int port, GuiceServletContextListener listener) {
        final Server server = new Server(port);
        final WebAppContext root = new WebAppContext();

        // DI
        root.addEventListener(listener);
        root.addFilter(GuiceFilter.class, "/*", null);

        root.setContextPath("/");
        root.setParentLoaderPriority(true);
        final String webappDirLocation = "src/main/webapp/";
        root.setDescriptor(webappDirLocation + "/WEB-INF/web.xml");
        root.setResourceBase(webappDirLocation);
        server.setHandler(root);

        return server;
      }

      public static void main(String[] args) throws Exception{
        int port = normalizePort(System.getenv("PORT"));

        GuiceServletContextListener listener = new GuiceServletContextListener() {

          @Override
          protected Injector getInjector() {
            return Guice.createInjector(new ProductionModule());
          }
        };
        Server server = createServer(port, listener);
        server.start();
        server.join();
      }
    }

In the code above, I've split out the port normalization, and server instantiation and configuration, to highlight the creation of a `GuiceServletContextListener` object, which configures a Guice injector to load our `ProductionModule`.

Next, configure our resource class to inject its dependencies:

    package com.example.featureswitchservice;

    import com.esotericsoftware.yamlbeans.YamlException;

    import javax.inject.Inject;
    import javax.inject.Named;
    import javax.ws.rs.GET;
    import javax.ws.rs.Path;
    import javax.ws.rs.Produces;
    import javax.ws.rs.QueryParam;
    import javax.ws.rs.core.MediaType;
    import java.io.FileNotFoundException;
    import java.util.Map;

    @Path("feature_switch_config")
    public class ConfigResource {
      @QueryParam("id") String id;
      @QueryParam("os") String os;
      @QueryParam("version") String version;

      Map<String, Map<String, String>> config;

      @Inject
      public ConfigResource(@Named("config") Map<String, Map<String, String>> config) {
        this.config = config;
      }

        @GET
        @Produces(MediaType.APPLICATION_JSON)
        public Map<String, Boolean> get() throws FileNotFoundException, YamlException {
          Input input = FeatureSelector.requireInput(new String[]{id, os, version});
          FeatureSelector selector = new FeatureSelector(this.config);
          Map<String, Boolean> selected = selector.select(input);
          return selected;
        }
    }

Note the `@Inject` annotation on our constructor. Also note the use of `@Named` to disambiguate which `Map` we should inject.

Now update the integration tests:

    package com.example.featureswitchservice;

    import com.esotericsoftware.yamlbeans.YamlException;
    import com.fasterxml.jackson.databind.ObjectMapper;
    import com.google.inject.Guice;
    import com.google.inject.Injector;
    import com.google.inject.Provides;
    import com.google.inject.servlet.GuiceServletContextListener;
    import com.google.inject.util.Modules;
    import com.sun.jersey.guice.JerseyServletModule;
    import org.eclipse.jetty.server.Server;
    import org.junit.After;
    import org.junit.Before;
    import org.junit.Test;

    import javax.inject.Named;
    import javax.ws.rs.client.Client;
    import javax.ws.rs.client.ClientBuilder;
    import javax.ws.rs.client.WebTarget;
    import java.io.FileNotFoundException;
    import java.io.IOException;
    import java.util.HashMap;
    import java.util.Map;

    import static org.junit.Assert.assertEquals;

    public class ConfigResourceTest {
      Server server;
      WebTarget target;

      @Before
      public void setUp() throws Exception {
        GuiceServletContextListener listener = new GuiceServletContextListener() {

          @Override
          protected Injector getInjector() {
            return Guice.createInjector(Modules.override(new ProductionModule())
                .with(new JerseyServletModule() {
                  @Provides
                  @Named("config")
                  Map<String, Map<String, String>> get() throws FileNotFoundException, YamlException {
                    return FeatureSelector.createFeatureMap();
                  }
                }));
          }
        };
        server = Main.createServer(Main.DEFAULT_PORT, listener);
        server.start();
        Client client = ClientBuilder.newClient();
        target = client.target(String.format("http://localhost:%d/", Main.DEFAULT_PORT));
      }

      @After
      public void tearDown() throws Exception {
        server.stop();
      }

      @Test
      public void testGet() throws IOException {
        final String json = target
            .path("feature_switch_config")
            .queryParam("id", "123")
            .queryParam("os", "android")
            .queryParam("version", "2.3")
            .request()
            .get(String.class);
        HashMap<String, Boolean> expected = new HashMap<>();
        expected.put("feature_c", true);
        ObjectMapper mapper = new ObjectMapper();
        Map<String, Boolean> actual = mapper.readValue(json, Map.class);
        assertEquals(expected, actual);
      }
    }

Observe that we're now instantiating a new _injector_, which overrides our config source. Again, we'll modify the source after completing Guice migration.

Also note that we're running the same server instantiated in `Main`. This allows us to integration test our service. We can also add unit tests for our resource class, but I'll defer that to a follow-up change, so this commit is focused just on migrating to Guice.

Modify _web.xml_ to use Guice for all DI:

    <?xml version="1.0" encoding="UTF-8"?>
    <web-app version="2.4" xmlns="http://java.sun.com/xml/ns/j2ee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_4.xsd">

        <!-- Minimal web.xml to bootstrap Guice. All the real config goes on inside GuiceConfig class. -->

        <listener>
            <display-name>GuiceConfig</display-name>
            <listener-class>com.example.featureswitchservice.GuiceListener</listener-class>
        </listener>

        <filter>
            <filter-name>guiceFilter</filter-name>
            <filter-class>com.google.inject.servlet.GuiceFilter</filter-class>
        </filter>

        <filter-mapping>
            <filter-name>guiceFilter</filter-name>
            <url-pattern>/*</url-pattern>
        </filter-mapping>
    </web-app>

I copied this [web.xml](https://raw.githubusercontent.com/cb372/jersey-jetty-guice-archetype/master/src/main/resources/archetype-resources/src/main/webapp/WEB-INF/web.xml) from the _Jersey-Jetty-Guice_ Maven archetype.

Finally, update _pom.xml_ to include the artifacts we need:

    ...
    <dependency>
        <groupId>com.fasterxml.jackson.jaxrs</groupId>
        <artifactId>jackson-jaxrs-json-provider</artifactId>
        <version>2.4.0</version>
    </dependency>
    <!--DI-->
    <dependency>
        <groupId>com.google.inject</groupId>
        <artifactId>guice</artifactId>
        <version>3.0</version>
    </dependency>
    <dependency>
        <groupId>com.sun.jersey.contribs</groupId>
        <artifactId>jersey-guice</artifactId>
        <version>1.18</version>
    </dependency>
    ...

We use _jackson-jaxrs-json-provider_ for rendering JSON, _guice_ for Guice, and _jersey-guice_ for Jersey-Guice integration.

Test via _curl_, run integration tests (`mvn test`), and commit.
