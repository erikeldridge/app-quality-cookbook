# Checkstyle

Checkstyle is effective, widely used, and can be run in an IDE and CI.

## Prerequisites

* Java
* Maven

## Install

Use Maven fetch and run checkstyle. Configure pom.xml:

```
...
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-checkstyle-plugin</artifactId>
            <version>2.14</version>
            <executions>
                <execution>
                    <id>checkstyle</id>
                    <phase>validate</phase>
                    <goals>
                        <goal>check</goal>
                    </goals>
                    <configuration>
                        <failOnViolation>true</failOnViolation>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
...
```

## Verify

```
$ mvn checkstyle:check
[INFO] Scanning for projects...
[INFO]
[INFO] ------------------------------------------------------------------------
[INFO] Building my-app 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO]
[INFO] --- maven-checkstyle-plugin:2.14:check (default-cli) @ my-app ---
...
```

## Upgrade Checkstyle

Edit pom.xml to use a more recent version (6.3):

```
...
<build>
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-checkstyle-plugin</artifactId>
                <version>2.14</version>
                <dependencies>
                    <dependency>
                        <groupId>com.puppycrawl.tools</groupId>
                        <artifactId>checkstyle</artifactId>
                        <version>6.3</version>
                    </dependency>
                </dependencies>
            </plugin>
        </plugins>
    </pluginManagement>
...
```

## Get Google's style rules:

Google has published its Java style guide and some kind folks have built a Checkstyle config to enforce it.

```
$ cd my-app/src
$ wget https://raw.githubusercontent.com/checkstyle/checkstyle/f0326fd4c85c3779c47013d2800ef6daf28721b1/src/main/resources/google_checks.xml
$ ls src/
google_checks.xml  main  test
```

Note: we're fetching a specific version that works with checkstyle 6.3

## Configure Checkstyle to use Google's rules:

```
...
<plugins>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <version>2.14</version>
        <configuration>
            ...
            <configLocation>src/google_checks.xml</configLocation>
        </configuration>
...
```
