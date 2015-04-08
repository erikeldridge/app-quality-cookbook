# Style check

Style checking tools give us a way to verify the code we've written conforms to a "style guide". This is awesome. Why? Because we can codify best-practices, such as [Google's Java style guide](https://google-styleguide.googlecode.com/svn/trunk/javaguide.html), and an objectively enforce them. This can save time and debate in code review.

We'll use [Checkstyle](https://github.com/checkstyle/checkstyle) for this exercise. Install it via Maven and configure it to [fail if there are any errors](https://maven.apache.org/plugins/maven-checkstyle-plugin/usage.html):

```xml
...
</build>
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <version>2.15</version>
        <executions>
            <execution>
                <id>validate</id>
                <phase>validate</phase>
                <configuration>
                    <configLocation>checkstyle.xml</configLocation>
                    <encoding>UTF-8</encoding>
                    <consoleOutput>true</consoleOutput>
                    <failsOnError>true</failsOnError>
                </configuration>
                <goals>
                    <goal>check</goal>
                </goals>
            </execution>
        </executions>
    </plugin>
</plugins>
</build>
...
```

Download the [Checkstyle rules for Google's Java style guide](http://checkstyle.sourceforge.net/google_style.html):

```nohighlight
$ wget https://raw.githubusercontent.com/checkstyle/checkstyle/master/src/main/resources/google_checks.xml
```

[Configure Checkstyle to use Google's style rules](https://maven.apache.org/plugins/maven-checkstyle-plugin/examples/custom-checker-config.html):

```xml
...
<configLocation>./google_checks.xml</configLocation>
...
```

By default, the Checkstyle plugin uses Checkstyle 6.1.1, but the Google style checks require a more recent version. [Upgrade the plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/examples/upgrading-checkstyle.html):

```xml
...
<build>
<pluginManagement>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-checkstyle-plugin</artifactId>
      <version>2.15</version>
      <dependencies>
        <dependency>
          <groupId>com.puppycrawl.tools</groupId>
          <artifactId>checkstyle</artifactId>
          <version>6.4.1</version>
        </dependency>
      </dependencies>
    </plugin>
  </plugins>
</pluginManagement>
...
```

Run it:

```nohighlight
$ mvn verify
```

By default, the style rules will only warn:

```nohighlight
...
[INFO] --- maven-checkstyle-plugin:2.15:check (validate) @ my-app ---
[INFO] Starting audit...
/home/vagrant/my-app/src/main/java/com/example/app/App.java:4: warning: First sentence should be present.
/home/vagrant/my-app/src/main/java/com/example/app/App.java:9:1: warning: '{' should be on the previous line.
/home/vagrant/my-app/src/main/java/com/example/app/App.java:10: warning: 'method def modifier' have incorrect indentation level 4, expected level should be 2.
/home/vagrant/my-app/src/main/java/com/example/app/App.java:11: warning: 'method def lcurly' have incorrect indentation level 4, expected level should be 2.
/home/vagrant/my-app/src/main/java/com/example/app/App.java:11:5: warning: '{' should be on the previous line.
/home/vagrant/my-app/src/main/java/com/example/app/App.java:12: warning: 'method def' child have incorrect indentation level 8, expected level should be 4.
/home/vagrant/my-app/src/main/java/com/example/app/App.java:13: warning: 'method def rcurly' have incorrect indentation level 4, expected level should be 2.
Audit done.
...

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

Edit the google_checks.xml you downloaded to change the _severity_ property from "warning" to "error":

```xml
...
  <property name="severity" value="error"/>
...
```

Re-run and observe the tests now fail:

```nohighlight
$ mvn test
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.298s
[INFO] Finished at: Mon Mar 23 00:32:57 UTC 2015
[INFO] Final Memory: 9M/23M
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-checkstyle-plugin:2.15:check (validate) on project my-app: Failed during checkstyle execution: There are 8 errors reported by Checkstyle 6.4.1 with ./google_checks.xml ruleset. -> [Help 1]
...
```

Fix any issues so the build succeeds and commit your changes:

```nohighlight
$ git add pom.xml
$ git add <any code that's changed>
$ git commit -m "Add checkstyle"
```

[Search Github for XML files containing "checkstyle"](https://github.com/search?l=xml&q=checkstyle&ref=searchresults&type=Code&utf8=%E2%9C%93) to see configuration files used by other projects.

Comparable tools: [Sonarqube](http://www.sonarqube.org/), [Uncrustify](https://github.com/bengardner/uncrustify), [pep8](https://github.com/jcrocholl/pep8).
