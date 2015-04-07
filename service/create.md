# Create a Java web service using Jersey

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