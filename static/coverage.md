# Test coverage

Test coverage tools help us understand how much of our code has been tested.

Tests have a cost, so it may be unreasonable to require 100% coverage, but we can use test coverage to highlight areas requiring attention.

We'll use a tool called [JaCoCo](http://www.eclemma.org/jacoco). Install it via Maven:

    <plugin>
        <groupId>org.jacoco</groupId>
        <artifactId>jacoco-maven-plugin</artifactId>
        <version>0.7.4.201502262128</version>
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

Run it:

    $ mvn verify
    ...
    [INFO] 
    [INFO] 
    [INFO] --- jacoco-maven-plugin:0.7.4.201502262128:check (default-check) @ my-app ---
    [INFO] Analyzed bundle 'my-app' with 1 classes
    [WARNING] Rule violated for bundle my-app: complexity covered ratio is 0.00, but expected minimum is 0.60
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD FAILURE
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 2.937s
    [INFO] Finished at: Tue Mar 24 09:03:08 UTC 2015
    [INFO] Final Memory: 15M/36M
    [INFO] ------------------------------------------------------------------------
    [ERROR] Failed to execute goal org.jacoco:jacoco-maven-plugin:0.7.4.201502262128:check (default-check) on project my-app: Coverage checks have not been met. See log for details. -> [Help 1]
    ...

The build should fail if _x_ percent our code is untested, where _x_ is a configuration option in the plugin. The configuration above fails unless at least 60% of code is tested:

    <limits>
        <limit>
            <counter>LINE</counter>
            <value>COVEREDRATIO</value>
            <minimum>0.60</minimum>
        </limit>
    </limits>

Run it:

    $ mvn verify
    
Tune the percentage threshold until the check passes for you. As you add tests you can increase the percentage required.