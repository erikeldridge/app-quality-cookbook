# Static analysis and continuous integration

The phrase "static analysis" refers to inspecting the aspects of software that do not change, like source code, byte code, etc., for correctness. Code is _static_ in that it doesn't change, as opposed to things like memory and network usage, which are _dynamic_ and change while software is running.

In simplistic terms, we perform static analysis when we read and review code. Our compiler performs static analysis when it parses our code. 

We'll explore four categories of static analysis tools in this section:
* Lint
* Style check
* Bug check
* Test coverage

We'll conclude by introducing continuous integration, which we can use to automatically perform tasks, like running tests and static analysis, in response to changes in a code base.

## Lint

The verb [lint](http://en.wikipedia.org/wiki/Lint_%28software%29), as applied to software, refers to scanning code for errors.

Java's compiler provides a relatively simple example:

    $ javac -Xlint:all Main.java

For example, define a class that prints "1" if you pass 1 or "2" if you pass 2:

    class Main {
        public static void main(String[] args) {
            switch (Integer.valueOf(args[0])) {
                case 1:
                    System.out.println("1");
                case 2:
                    System.out.println("2");
            }
        }
    }

Compile it:

    $ javac Main.java

Run it:

    $ java Main 2
    2
    $ java Main 1
    1
    2

Hmm. That's a bug.

Java's [switch statement](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html) "falls through" cases unless you add a _break_ statement.

Lint could have helped:

    $ javac -Xlint:all Main.java
    Main.java:6: warning: [fallthrough] possible fall-through into case
        case 2:
        ^
    1 warning

I think linters are worth being aware of because many languages have them, eg [Android's lint](http://developer.android.com/tools/help/lint.html), [PyLint](http://www.pylint.org/), [JSLint](http://www.jslint.com/)/[JSHint](http://jshint.com/), etc.

## Style check

Style checking tools give us a way to verify the code we've written conforms to a "style guide". This is awesome. Why? Because we can codify best-practices, such as [Google's Java style guide](https://google-styleguide.googlecode.com/svn/trunk/javaguide.html), and an objectively enforce them. This can save time and debate in code review.

We'll use [Checkstyle](https://github.com/checkstyle/checkstyle) for this exercise. Install it via Maven and configure it to [fail if there are any errors](https://maven.apache.org/plugins/maven-checkstyle-plugin/usage.html):

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

Download the [Checkstyle rules for Google's Java style guide](http://checkstyle.sourceforge.net/google_style.html):

    $ wget https://raw.githubusercontent.com/checkstyle/checkstyle/master/src/main/resources/google_checks.xml

[Configure Checkstyle to use Google's style rules](https://maven.apache.org/plugins/maven-checkstyle-plugin/examples/custom-checker-config.html) instead of checkstyle.xml:

    ...
    <configLocation>./google_checks.xml</configLocation>
    ...

By default, the Checkstyle plugin uses Checkstyle 6.1.1, but the Google style checks require a more recent version. [Upgrade the plugin](https://maven.apache.org/plugins/maven-checkstyle-plugin/examples/upgrading-checkstyle.html):

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

Run it:

    $ mvn verify

By default, the style rules will only warn:

    ...
    [INFO] --- maven-checkstyle-plugin:2.15:check (validate) @ red-camping-run ---
    [INFO] Starting audit...
    /home/vagrant/red-camping-run/src/main/java/com/example/redcampingrun/App.java:4: warning: First sentence should be present.
    /home/vagrant/red-camping-run/src/main/java/com/example/redcampingrun/App.java:9:1: warning: '{' should be on the previous line.
    /home/vagrant/red-camping-run/src/main/java/com/example/redcampingrun/App.java:10: warning: 'method def modifier' have incorrect indentation level 4, expected level should be 2.
    /home/vagrant/red-camping-run/src/main/java/com/example/redcampingrun/App.java:11: warning: 'method def lcurly' have incorrect indentation level 4, expected level should be 2.
    /home/vagrant/red-camping-run/src/main/java/com/example/redcampingrun/App.java:11:5: warning: '{' should be on the previous line.
    /home/vagrant/red-camping-run/src/main/java/com/example/redcampingrun/App.java:12: warning: 'method def' child have incorrect indentation level 8, expected level should be 4.
    /home/vagrant/red-camping-run/src/main/java/com/example/redcampingrun/App.java:13: warning: 'method def rcurly' have incorrect indentation level 4, expected level should be 2.
    Audit done.
    ...
    
    Results :
    
    Tests run: 1, Failures: 0, Errors: 0, Skipped: 0
    
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------

Edit the google_checks.xml you downloaded to change the "severity" property from "warning" to "error":

    ...
      <property name="severity" value="error"/>
    ...

Re-run and observe the tests now fail:

    $ mvn test
    ...
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD FAILURE
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 1.298s
    [INFO] Finished at: Mon Mar 23 00:32:57 UTC 2015
    [INFO] Final Memory: 9M/23M
    [INFO] ------------------------------------------------------------------------
    [ERROR] Failed to execute goal org.apache.maven.plugins:maven-checkstyle-plugin:2.15:check (validate) on project red-camping-run: Failed during checkstyle execution: There are 8 errors reported by Checkstyle 6.4.1 with ./google_checks.xml ruleset. -> [Help 1]
    ...

Fix each of the issues so the build succeeds and commit your changes:

    $ git add pom.xml
    $ git add <any code that's changed>
    $ git commit -m "Add checkstyle"

[Search Github for XML files containing "checkstyle"](https://github.com/search?l=xml&q=checkstyle&ref=searchresults&type=Code&utf8=%E2%9C%93) to see configuration files used by other projects.

Comparable tools: [Sonarqube](http://www.sonarqube.org/), [Uncrustify](https://github.com/bengardner/uncrustify), [pep8](https://github.com/jcrocholl/pep8).

## Bug check

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

For example, add the following code to your class to fail the [_strings_ rule](http://pmd.sourceforge.net/pmd-5.2.3/pmd-java/rules/java/strings.html) for StringInstantiation:

    String s = new String("this is a new string");

As with Checkstyle, we've configured PMD to run during the verification phase of the Maven build:

    $ mvn verify

If you played with failing string code above you can easily undo the change by checking out your code from git:

    $ git checkout -- src/main/java/com/example/app/App.java

And then commit your pom.xml:

    $ git add pom.xml
    $ git commit -m "Add PMD"

[FindBugs](http://findbugs.sourceforge.net/index.html) is a very common, comparable tool that analyzes compiled byte code looking for bugs. We only covered PMD here to limit the complexity of this exercise, but you can set up FindBugs in much the same way: install via Maven plugin, configure, etc.

False alerts are a bane of static analysis. Each project is configurable, individual tests can be enabled, modified, or disabled, but this requires works. [Sonarqube](http://www.sonarqube.org/) is one project working to improve the accuracy of bug finding tools.

## Test coverage

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
    [INFO] --- jacoco-maven-plugin:0.7.4.201502262128:check (default-check) @ red-camping-run ---
    [INFO] Analyzed bundle 'red-camping-run' with 1 classes
    [WARNING] Rule violated for bundle red-camping-run: complexity covered ratio is 0.00, but expected minimum is 0.60
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD FAILURE
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 2.937s
    [INFO] Finished at: Tue Mar 24 09:03:08 UTC 2015
    [INFO] Final Memory: 15M/36M
    [INFO] ------------------------------------------------------------------------
    [ERROR] Failed to execute goal org.jacoco:jacoco-maven-plugin:0.7.4.201502262128:check (default-check) on project red-camping-run: Coverage checks have not been met. See log for details. -> [Help 1]
    ...

The build should fail if _x_ percent our code is untested, where _x_ is a configuration option in the plugin. The configuration above fails unless at least 60% of code is tested:

    <limits>
        <limit>
            <counter>COMPLEXITY</counter>
            <value>COVEREDRATIO</value>
            <minimum>0.60</minimum>
        </limit>
    </limits>

## Continuous integration

[Continuous integration](http://www.thoughtworks.com/continuous-integration) (CI) is the practice of testing and merging code continuously, as opposed to waiting until development completes. We can use CI to run our static analysis on a change in review to see if it will "break the build" and on each commit to verify the health of the project. This can help us catch issues early.

We'll use a tool called [Travis](https://travis-ci.org/) for a few reasons: it's relatively modern, so it's enjoyable to use; it integrates seamlessly with Github; it's freely available for open repositories like ours.

Before diving in, I should call out a comparable tool called [Jenkins](https://jenkins-ci.org/). Jenkins, and its predecessor Hudson, are widely used, solid, open-source CI servers, which you'll see in lots of documentation and workplaces. We're not using it here because it's relatively difficult to set up and use, but we should be aware of it.

Ok. Here we go. Log into [travis-ci.org](https://travis-ci.org/) (Note: .org, not .com, which we'd use for closed source projects) with your Github account and allow it to manage your Github repositories.

Travis fetch and display your Github repositories. Find the repository you're using for this exercise and enable CI for it by toggling the switch on the right of the dashboard. Click "Travis CI" in the upper left to navigate home, and then use the "Search all repositories" box to find your repository. Click the repository name to open the repository's dashboard. The url will be  https://travis-ci.org/&lt;your username&gt;/&lt;your repository name&gt;.

In your local repository, define a .travis.yml config file:

    language: java
    jdk:
        - openjdk7

You can use [Travis' linter](http://lint.travis-ci.org/) to verify your config is correct.

Commit your config file:

    $ git add .travis.yml
    $ git commit -m "Define new travis config file"

Push your change to Github to trigger CI:

    $ git push origin master

Navigate back to the project dashboard you opened above.

You should see your commit listed under the "Current" tab. It will be yellow until the build passes. The entry will turn green if the build passes or red if the build fails.

After the build completes, review the logged output. Look for the Maven commands Travis ran and try to run them locally:

    $ mvn install -DskipTests=true -Dmaven.javadoc.skip=true -B -V
    ...
    $ mvn test -B

Observe that Travis defaults to Maven for Java projects.

Modify your config file to verify our build:

    language: java
    jdk:
      - openjdk7
    script: mvn verify

Commit this change and push. Observe Travis now runs `mvn verify` after installation instead of `mvn test -b`.

Define a pre-push githook to run these commands before pushing to github:

    mvn install -DskipTests=true -Dmaven.javadoc.skip=true -B -V
    installed=$?
    
    mvn verify
    verified=$?
    
    if [ ! $installed ] || [ ! $verified ]
    then
        exit 1
    fi

This will help you catch errors before they break the build.

A quick aside: defining a githook to avoid wasting CI time is an example of progressive testing - our tests should become progressively more "expensive" as we get closer to shipping a product.

In development, we want immediate feedback, like IDE hints, lint errors, and quick unit tests to ensure our change works. To avoid breaking the build, but still provide on-demand feedback, we can have CI run full unit and integration test suites before a change merges.

CI can periodically run still more expensive functional tests to ensure our project is healthy on an ongoing basis (because we always want the freedom to apply a critical bug fix on master and deploy).

To simulate production before we ship, we can run live integration tests with real data, and define pre-release versions, eg "nightly", alpha", "beta", etc. 

Delivery itself is the final test, which can "cost" us customers if a product doesn't work.

Ok. Let's create a code review and observe CI's validation on the review.

Create and checkout a new branch:

    $ git checkout -b edit_readme

Create and commit a change:

    $ echo "asd" >> README.md
    $ git add README.md
    $ git commit -m "Add text to readme"

Push the branch to Github:

    $ git push origin edit_readme

Navigate to your repo in Github. Click the "Pull requests" link on the right. Click the "New pull request" button to create a pull request. Set master as the base and the branch you just pushed, eg "edit_readme", as the branch to compare.

After creating the pull request, observe Github has added a section it with the Travis details. While the build is in progress, you'll see "Waiting to hear about &lt;git sha&gt;." This status will change to "All is well" when the build succeeds.

Navigate to the Travis dashboard. Observe there's a build for "&lt;your branch&gt; Add text to readme." Click the "Pull requests" tab and observe there's an entry for your pull request.

Once your build passes, merge the change on Github. Observe Travis starts a new build on the master branch to verify "Merge pull request #1 from &lt;your github user name&gt;/&lt;your branch name&gt;".

Congrats! You've just used CI to run static analysis and tests before and after merging.

To learn more about Travis:
* see [Travis' getting started documentation](http://docs.travis-ci.com/user/getting-started/) for config examples
* see [Travis' java docs](http://docs.travis-ci.com/user/languages/java/) for more configuration options
* [search Github for .travis.yml files](https://github.com/search?utf8=%E2%9C%93&q=%22language%3A+java%22+language%3Ayml&type=Code&ref=searchresults) to see how other projects use Travis.

## Conclusion

We've covered linters, style checkers, bug finders, test covereage tools, and a means to run them all on each commit via CI. We've also introduced alternative tools so you can continue exploring this space, compare, and find the right approaches for your projects.

Programmatic quality control is much more consistent, but also more brittle, than manual quality control, so we use a balance of both. In general, start with some basic defaults, like the Google style guide, and enforce them programmatically. As you see recurring issues, think about how you could automate the steps you take to prevent them.

