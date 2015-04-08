# Bug check

[Cyclomatic complexity](http://en.wikipedia.org/wiki/Cyclomatic_complexity) refers to the number of paths through a given piece of software. For example, the following function has a complexity value of 1:

    int add(int a, int b) {
        return a + b;
    }

In comparison, a function with conditional logic is relatively complex because we have to consider multiple code paths:

    int getLength(String str) {
        if (str == null) {
            return 0;
        } else {
            return str.length();
        }
    }

We can use static analysis tools to evaluate complexity and detect other operational issues. For this exercise, we'll use [PMD](http://pmd.sourceforge.net/pmd-5.2.3/).

To paraphrase PMD's site, PMD scans Java source code for dead, suboptimal, or duplicate code, possible bugs and overcomplicated expressions.

Install via Maven:

    ...
    <build>
          ...
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-pmd-plugin</artifactId>
              <version>3.4</version>
              <executions>
                  <execution>
                      <phase>verify</phase>
                      <goals>
                          <goal>check</goal>
                      </goals>
                  </execution>
              </executions>
              <configuration>
                  <verbose>true</verbose>
                  <rulesets>
                      <ruleset>/rulesets/java/braces.xml</ruleset>
                      <ruleset>/rulesets/java/codesize.xml</ruleset>
                      <ruleset>/rulesets/java/strings.xml</ruleset>
                      <ruleset>/rulesets/java/unusedcode.xml</ruleset>
                  </rulesets>
                  <failOnViolation>true</failOnViolation>
              </configuration>
          </plugin>
      </plugins>
    </build>
    ...

The _execution_ block tells Maven to run this check as part of the verification phase.

The _verbose_ configuration tells Maven to print PMD's output.

The _rulesets_ configuration defines which rules to apply. See all the rulesets and examples of failing code on [PMD's rulesets documentation](http://pmd.sourceforge.net/pmd-5.2.3/pmd-java/rules/index.html). See all ruleset names in [PMD's ruleset definitions](https://github.com/pmd/pmd/tree/master/pmd-java/src/main/resources/rulesets/java).

For example, add the following code to your class to fail the [_strings_ rule](http://pmd.sourceforge.net/pmd-5.2.3/pmd-java/rules/java/strings.html) for _StringInstantiation_:

    String s = new String("this is a new string");

As with Checkstyle, we've configured PMD to run during the verification phase of the Maven build:

    $ mvn verify

If you played with failing string code above you can easily undo the change by checking out your code from git:

    $ git checkout -- src/main/java/com/example/app/App.java

And then commit your pom.xml:

    $ git add pom.xml
    $ git commit -m "Add PMD"

[FindBugs](http://findbugs.sourceforge.net/index.html) is a very common, comparable tool that analyzes byte code looking for bugs. We only covered PMD here to limit the complexity of this exercise, but you can set up FindBugs in much the same way: install via Maven plugin, configure, etc.

False alerts are a bane of static analysis. Each project is configurable, individual tests can be enabled, modified, or disabled, but this requires work. [Sonarqube](http://www.sonarqube.org/) is one project working to [improve the accuracy](http://www.sonarqube.org/already-158-checkstyle-and-pmd-rules-deprecated-by-sonarqube-java-rules/) of bug finding tools.
