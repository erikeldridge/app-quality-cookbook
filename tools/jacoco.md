# JaCoCo

JaCoCo is a popular code coverage tool

## Prerequisites

* Java

## Install

Install via Maven. Configure pom.xml:

```
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
```

### Verify

Run and observe code coverage output:
```
$ mvn verify
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

* [JaCoCo's website](http://www.eclemma.org/jacoco/)
* [JaCoCo's maven plugin](http://www.eclemma.org/jacoco/trunk/doc/maven.html)