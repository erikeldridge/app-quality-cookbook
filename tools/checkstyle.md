# Checkstyle

Checkstyle is effective, widely used, and can be run in an IDE and CI.

## Prerequisites

* Java
* Maven

## Install

Get Google's style rules:

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

### Verify

```
$ mvn validate
...
Downloading: http://repo.maven.apache.org/maven2/org/apache/maven/plugins/maven-checkstyle-plugin/2.14/maven-checkstyle-plugin-2.14.pom
...
```
