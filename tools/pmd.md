# PMD

PMD provides a robust complexity analysis that is relatively easy to configure.

## Install

Use Maven to install and run. Edit pom.xml:

```
...
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-pmd-plugin</artifactId>
      <version>3.4</version>
      <configuration>
        <printFailingErrors>true</printFailingErrors>
        <verbose>true</verbose>
        <failurePriority>4</failurePriority>
        <rulesets>
           <ruleset>/rulesets/java/codesize.xml</ruleset>
        </rulesets>
      </configuration>
      <executions>
        <execution>
          <phase>verify</phase>
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

Note: the "ruleset" format is java-<ruleset file name>. For example, pass "java-strings" to use [java/strings.html](http://pmd.sourceforge.net/pmd-5.2.3/pmd-java/rules/java/codesize.html).

## Verify

```
$ mvn pmd:check
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building my-app 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] >>> maven-pmd-plugin:3.4:check (default-cli) @ my-app >>>
[INFO] 
[INFO] --- maven-pmd-plugin:3.4:pmd (pmd) @ my-app ---
[WARNING] Unable to locate Source XRef to link to - DISABLED
[WARNING] File encoding has not been set, using platform encoding UTF-8, i.e. build is platform dependent!
[INFO] 
[INFO] <<< maven-pmd-plugin:3.4:check (default-cli) @ my-app <<<
[INFO] 
[INFO] --- maven-pmd-plugin:3.4:check (default-cli) @ my-app ---
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
```

## Related

* [PMD rulesets](http://pmd.sourceforge.net/pmd-5.2.3/pmd-java/rules/index.html)
* [Maven PMD plugin](http://maven.apache.org/plugins/maven-pmd-plugin/)
