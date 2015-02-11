# Service

We need a simple service with which we can practice practice quality engineering

## Prerequisites

* Java
* Maven
* Unix

## Steps

### Initialize project

Use maven to generate a standard project structure:

```
mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

### Definition

Edit the generated App class to look like this:

```
public class App extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) {
        try {
            resp.getWriter().print("hi");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    public static void main(String[] args) throws Exception {
        Server server = new Server(8080);
        ServletContextHandler context = new ServletContextHandler(ServletContextHandler.SESSIONS);
        context.setContextPath("/");
        server.setHandler(context);
        context.addServlet(new ServletHolder(new App()),"/*");
        server.start();
        server.join();
    }
}
```

Edit the generated pom.xml file to include Jetty support:

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.example</groupId>
	<artifactId>service</artifactId>
	<packaging>jar</packaging>
	<version>1.0-SNAPSHOT</version>
	<name>service</name>
	<url>http://maven.apache.org</url>
	<dependencies>
	<dependency>
	  <groupId>junit</groupId>
	  <artifactId>junit</artifactId>
	  <version>3.8.1</version>
	  <scope>test</scope>
	</dependency>
	  <dependency>
	      <groupId>org.eclipse.jetty</groupId>
	      <artifactId>jetty-servlet</artifactId>
	      <version>9.3.0.M1</version>
	  </dependency>
	  <dependency>
	      <groupId>javax.servlet</groupId>
	      <artifactId>javax.servlet-api</artifactId>
	      <version>3.1.0</version>
	  </dependency>
	</dependencies>
	<build>
	    <plugins>
	        <plugin>
	        	<!-- use 1.7 to support annotations -->
	            <artifactId>maven-compiler-plugin</artifactId>
	            <version>3.2</version>
	            <configuration>
	                <source>1.7</source>
	                <target>1.7</target>
	            </configuration>
	        </plugin>
	        <plugin>
	        	<!-- export dependencies -->
	            <groupId>org.apache.maven.plugins</groupId>
	            <artifactId>maven-dependency-plugin</artifactId>
	            <version>2.4</version>
	            <executions>
	                <execution>
	                    <id>copy-dependencies</id>
	                    <phase>package</phase>
	                    <goals><goal>copy-dependencies</goal></goals>
	                </execution>
	            </executions>
	        </plugin>
	    </plugins>
	</build>
</project>

```

### Dev env

In IntelliJ:
1. Select "Import project" 
1. Select your pom.xml
1. Select all the defaults
1. After the project is created, open App.java
1. Select Build > compile
1. Observe success
1. Open AppTest.java
1. Select Run > Run ...
1. Select AppTest
1. Observe test window shows all green

### Run

1. Open terminal
1. Run:

```
java -cp target/classes:target/dependency/* com.example.App
```

1. Observe server starts:

```
...
2015-02-05 09:28:13.978:INFO:oejs.Server:main: jetty-9.3.0.M1
2015-02-05 09:28:14.067:INFO:oejsh.ContextHandler:main: Started o.e.j.s.ServletContextHandler@3b9a5f{/,null,AVAILABLE}
2015-02-05 09:28:14.108:INFO:oejs.ServerConnector:main: Started ServerConnector@ea7776{HTTP/1.1,[http/1.1]}{0.0.0.0:8888}
2015-02-05 09:28:14.108:INFO:oejs.Server:main: Started @413ms
```

1. View: http://localhost:8888

## Related

* [HttpServlet documentation](http://docs.oracle.com/cd/E17802_01/products/products/servlet/2.5/docs/servlet-2_5-mr2/javax/servlet/http/HttpServlet.html)
* [Jetty documentation](http://www.eclipse.org/jetty/documentation/current/)
* [Maven quickstart](http://maven.apache.org/guides/getting-started/maven-in-five-minutes.html)