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

Observe we're still loading from our resource file.

Another advantage of DI is [singleton](http://sourcemaking.com/design_patterns/singleton) management. Singletons can be very difficult to test, especially if they are implemented using global variables.

Next, modify our main function to inject these dependencies.

