# Code coverage

## Goals

* Identify an appropriate tool
* Generate and interpret a code coverage report

## Motivation

Code coverage analysis can tell us how many lines of our code are touched by our tests.

Having 100% coverage doesn't guarantee freedom from bugs, in part because our tests don't cover all usage patterns, but it can help us identify under-maintained portions of our code.

## Steps

### Tooling

An appropriate tool should:
* be runnable from the command line
* be runnable in an IDE
* generate machine-readable output for CI

### Using JaCoCo in Maven

Incorporate JaCoCo's plugin:
```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.example.foo</groupId>
  <artifactId>foo</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>foo</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
    <build>
        <plugins>
            <plugin>
                <groupId>org.jacoco</groupId>
                <artifactId>jacoco-maven-plugin</artifactId>
                <version>0.7.2.201409121644</version>
                <executions>
                <execution>
                    <id>default-prepare-agent</id>
                    <goals>
                        <goal>prepare-agent</goal>
                    </goals>
                </execution>
                <execution>
                    <id>default-report</id>
                    <phase>prepare-package</phase>
                    <goals>
                        <goal>report</goal>
                    </goals>
                </execution>
                    <execution>
                        <id>default-check</id>
                        <goals>
                            <goal>check</goal>
                        </goals>
                        <configuration>
                            <rules>
                                <rule>
                                    <element>BUNDLE</element>
                                    <limits>
                                        <limit>
                                            <counter>COMPLEXITY</counter>
                                            <value>COVEREDRATIO</value>
                                            <minimum>0.60</minimum>
                                        </limit>
                                    </limits>
                                </rule>
                            </rules>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>
</project>
```

Run:
```
$ mvn clean verify
...
[INFO] --- jacoco-maven-plugin:0.7.2.201409121644:check (default-check) @ foo ---
[INFO] Analyzed bundle 'foo' with 1 classes
[WARNING] Rule violated for bundle foo: complexity covered ratio is 0.00, but expected minimum is 0.60
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
...
```

## Related

* [IntelliJ's Code Coverage documentation](https://www.jetbrains.com/idea/help/code-coverage.html)
* [JaCoCo's maven plugin documentation](http://www.eclemma.org/jacoco/trunk/doc/maven.html)

