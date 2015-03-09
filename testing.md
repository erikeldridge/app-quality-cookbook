# Testing

More than the act of testing, the act of designing tests is one of the best bug preventers known. The thinking that must be done to create a useful test can discover and eliminate bugs before they are coded. – Boris Beizer

In this exercise, we’ll explore three commonly used, broad categories of testing:
* unit
* integration
* functional

Unit tests assert correct behavior for small “units” of code.

Integration tests assert correct behavior for a composition of units.

Functional tests assert correct behavior of an entire product.

As Martin Fowler describes in his [Test Pyramid](http://martinfowler.com/bliki/TestPyramid.html) article, we should build on a base of unit tests, as these are the easiest to reason about and the fastest to run.

We’ll work with the code review group generator we created earlier. In the context of this exercise, a unit could be a function that splits our team into groups of three developers; an integration test could cover assignment of a person to a group; a functional test could verify the printed output is correctly formatted.

Since we wrote our code before writing the tests, we’ll apply tests in reverse order: functional, integration, units. This is actually a best-practice: when working with code that may need to be “refactored”, i.e., changed structurally without affecting the output, write tests first to help verify the program works the same before and after the change.

## Functional testing

We’ll build on our Unix experience for this and use the Bash scripting tool to collect the output from our program and assert its correctness.

### A quick dive into Bash

You can run Bash commands directly in your terminal. You can also save the commands in a file and then run the file.

#### Variables

    $ foo=bar
    $ echo $foo
    bar

#### Operators

    $ i=$((1 + 1)) # addition
    $ echo $i
    2

#### Loops

    $ for ((i=0; i<5; i++)); do echo $i; done
    0
    1
    2
    3
    4

#### Conditions

    $ i=1; if [[ $i = 1 ]]; then echo true; else echo false; fi

#### Functions

    $ function foo () { echo $1; };
    $ foo bar
    bar

### Define test

The code below assumes your review group generator runs like this:

    $ java Main e 2
    [b, f, e]

Our generator should always return the same value given the same inputs. We can assert the code is working correctly by verifying this.

Create a file for running our test:

    $ vim test.sh

In this file, add the command to run your generator and compare the output with known valid value:

    function assert () {
      if [[ "$1" = "$2" ]]; then
        echo success
        exit 0
      else
        echo error: $1 does not equal $2
        exit 1
      fi
    }

    actual=`java Main e 2`
    expected="[b, f]"
    
    assert "$actual" "$expected"

Note that the value we’re asserting is incorrect. We’ll run the failing test first to make sure the test actually works:

    $ bash test.sh 
    "[b, f, e] does not equal [b, f@]"

Fix the value and re-run:

    $ bash test.sh 
    success

With this test we can easily verify the output hasn’t changed. And we know from looking at the output that it’s correct. We can use integration and unit testing to assert correctness of the code that generates this output.

Now would be a good time to cut a branch and commit your changes:

    $ git branch exercise_2
    $ git checkout exercise_2
    $ git status
    $ git add test.sh
    $ git commit -m "define a script for functional testing"

## Integration testing

The requirements of the code generator are:

* accept a username and an assignment number
* generate a random group of three people from your larger team, including the given username
* passing any name from a group should return the same group for a given assignment number

Assuming our review group generator looks something like:

    public class Main {
        List<String> names;
    
        public Main(List<String> names){
            this.names = names;
        }
    
        public ArrayList<String> createGroup(String targetName) throws Exception {
            ArrayList<ArrayList<String>> groups = new ArrayList<>();
            ArrayList<String> tmpGroup = new ArrayList<>();
            for (String name: this.names) {
                tmpGroup.add(name);
                if (tmpGroup.size() == 3) {
                    groups.add(tmpGroup);
                    tmpGroup = new ArrayList<String>();
                }
            }
    
            for (ArrayList<String> group : groups) {
                if (group.contains(targetName)) {
                    return group;
                }
            }
    
            throw new Exception("target name not found");
        }
    
        public static void main( String[] args ) throws Exception {
            String targetName = args[0];
            long assignmentNumber = Long.valueOf(args[1]);
    
            List<String> names = new ArrayList<String>();
            names.add("a");
            names.add("b");
            names.add("c");
            Collections.shuffle(names, new Random(assignmentNumber));
    
            Main main = new Main(names);
    
            System.out.print(main.createGroup(targetName));
        }
    }

The main function above employs a few things to improve testability:

* IO, eg user input and printed output, is difficult to test. For example, we had to create a Bash script to assert correct output. By encapsulating IO in the main function, we can keep everything else pure expressions, in terms of [functional programming](http://en.wikipedia.org/wiki/Functional_programming), which are much easier to test
* Passing the list of shuffled names into our class touches on a concept called [dependency injection](http://en.wikipedia.org/wiki/Dependency_injection) and enables us to control the shuffling in our test environment
* We don’t need to test Collections.shuffle because we’re importing it from an external library and can assume it has been tested works correctly

We can use [JUnit](http://junit.org/) to test `createGroup` and verify it’s output satisfies the requirements.

We'll use Maven to simplify JUnit installation.

### A quick dive into Maven

Maven simplifies and standardizes build configuration. It is commonly used, and other tools like Gradle can pull from Maven repositories.

#### Install

    $ sudo apt-get install maven
    ...
    $ mvn -version
    Apache Maven 3.0.5
    Maven home: /usr/share/maven
    Java version: 1.7.0_75, vendor: Oracle Corporation
    Java home: /usr/lib/jvm/java-7-openjdk-i386/jre
    Default locale: en_US, platform encoding: UTF-8
    OS name: "linux", version: "3.13.0-45-generic", arch: "i386", family: "unix"

#### Create quickstart project

    $ mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ...
    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    [INFO] ------------------------------------------------------------------------
    [INFO] Total time: 29.494s
    [INFO] Finished at: Tue Feb 10 05:28:29 UTC 2015
    [INFO] Final Memory: 10M/25M
    [INFO] ------------------------------------------------------------------------
    ...
    $ ls -R my-app/
    my-app/:
    pom.xml  src

    my-app/src:
    main  test

    my-app/src/main:
    java

    my-app/src/main/java:
    com

    my-app/src/main/java/com:
    mycompany

    my-app/src/main/java/com/mycompany:
    app

    my-app/src/main/java/com/mycompany/app:
    App.java

    my-app/src/test:
    java

    my-app/src/test/java:
    com

    my-app/src/test/java/com:
    mycompany

    my-app/src/test/java/com/mycompany:
    app

    my-app/src/test/java/com/mycompany/app:
    AppTest.java

#### Create Maven-based IntelliJ project

1. Click "import project"
1. Select the pom.xml file created above
1. Once the project is created, copy your generator code into App.java and rename your class to "App"
1. Build your project and verify the tests created by Maven in AppTest.java still pass.
1. Modify it to conform to the latest JUnit (4.12) by editing your pom.xml to match JUnit’s installation documentation:
        ...
          <dependencies>
            <dependency>
              <groupId>junit</groupId>
              <artifactId>junit</artifactId>
              <version>4.12</version>
              <scope>test</scope>
            </dependency>
          </dependencies>
        ...

1. Edit AppTest.java to match JUnit’s getting started documentation:
        package com.mycompany.my-app;
        
        import static org.junit.Assert.assertEquals;
        
        import org.junit.Test;
        import org.junit.Ignore;
        import org.junit.runner.RunWith;
        import org.junit.runners.JUnit4;
        
        public class AppTest {
        
            @Test
            public void thisAlwaysPasses() {
        
            }
        
            @Test
            @Ignore
            public void thisIsIgnored() {
            }
        }

1. Verify the tests pass:
        $ mvn test
        ...
        
        -------------------------------------------------------
         T E S T S
        -------------------------------------------------------
        Running com.mycompany.app.AppTest
        Tests run: 2, Failures: 0, Errors: 0, Skipped: 1, Time elapsed: 0.052 sec
        
        Results :
        
        Tests run: 2, Failures: 0, Errors: 0, Skipped: 1
        ...

1. Commit these changes:
        $ git add pom.xml src/test
        $ git status
        $ git commit -m “upgrade JUnit to 4.12”

### Write integration tests

1. Edit AppTest.java to verify the output of `createGroup` contains 3 members:

        @Test
        public void groupsHaveThreePeople() {
            List<String> names = new ArrayList<String>();
            names.add("a");
            names.add("b");
            names.add("c");
            names.add("d");
            names.add("e");
            names.add("f");
            Generator generator = new App(names);
            List<String> group = generator.createGroup("a");
            assertEquals(group.size(), 3);
        }

1. Verify the group contains the given username:

        @Test
        public void givenUsernameIsInGroup() {
            List<String> names = new ArrayList<String>();
            names.add("a");
            names.add("b");
            names.add("c");
            names.add("d");
            names.add("e");
            names.add("f");
            Generator generator = new App(names);
            String name = "a";
            List<String> group = generator.createGroup(name);
            assertEquals(group.contains(name), true);
        }

1. Verify the group is consistent for all members of the group:

        @Test
        public void groupsHaveConsistentMembership() {
            List<String> names = new ArrayList<String>();
            names.add("a");
            names.add("b");
            names.add("c");
            names.add("d");
            names.add("e");
            names.add("f");
            Generator generator = new App(names);
            List<String> actualA = generator.createGroup("a");
            List<String> actualB = generator.createGroup("b");
            List<String> actualC = generator.createGroup("c");
            List<String> expected = new ArrayList<String>();
            expected.add("a");
            expected.add("b");
            expected.add("c");
            assertEquals(actualA, expected);
            assertEquals(actualB, expected);
            assertEquals(actualC, expected);
        }

1. Commit your changes:

        $ git add src/test
        $ git status
        $ git commit -m "define integration tests"

## Unit testing

We've used integration testing to verify `createGroup` works as a whole. We can use unit testing to assert each part of `createGroup` works correctly. We're currently working backward, so this is anticlimactic, but in our future changes we'll start with unit tests and you'll see they make all our other tests much easier to construct.

Assuming the `createGroup` function looks like this:

    public ArrayList<String> createGroup(String targetName) throws Exception {
        ArrayList<ArrayList<String>> groups = new ArrayList<>();
        ArrayList<String> tmpGroup = new ArrayList<>();
        for (String name: this.names) {
            tmpGroup.add(name);
            if (tmpGroup.size() == 3) {
                groups.add(tmpGroup);
                tmpGroup = new ArrayList<String>();
            }
        }

        for (ArrayList<String> group : groups) {
            if (group.contains(targetName)) {
                return group;
            }
        }

        throw new Exception("target name not found");
    }

Conceptually, it's doing a couple things:

* breaking a group up into subgroups of three people
* finding the subgroup containing a given user

Use this conceptual approach to extract two units of functionality:

    public ArrayList<ArrayList<String>> createSubgroups() {
        ArrayList<ArrayList<String>> groups = new ArrayList<>();
        ArrayList<String> tmpGroup = new ArrayList<>();
        for (String name: this.names) {
            tmpGroup.add(name);
            if (tmpGroup.size() == 3) {
                groups.add(tmpGroup);
                tmpGroup = new ArrayList<String>();
            }
        }
        return groups;
    }

    public ArrayList<String> findGroup(ArrayList<ArrayList<String>> groups, String name) {
        for (ArrayList<String> group : groups) {
            if (group.contains(name)) {
                return group;
            }
        }
        return null;
    }

The createGroup function now looks like this:

    public ArrayList<String> createGroup(String name) throws Exception {
        ArrayList<ArrayList<String>> groups = createSubgroups()
        ArrayList<String> group = findGroup(groups, name)
        if (group == null) {
            throw new Exception("target name not found");
        } else {
            return group
        }
    }

Run the functional and integration tests to assert everything still works.

Commit your changes (eg "extract createSubgroups and findGroup methods").

We can now run a couple of our tests at a lower level:

    @Test
    public void createSubgroups() {
        List<String> names = new ArrayList<String>();
        names.add("a");
        names.add("b");
        names.add("c");
        names.add("d");
        names.add("e");
        names.add("f");
        Generator generator = new Generator(names);
        ArrayList<ArrayList<String>> groups = generator.createSubgroups();
        assertEquals(groups.size(), 2);
        assertEquals(groups.get(0).size(), 3);
    }

    @Test
    public void findGroup() {
        List<String> names = new ArrayList<String>();
        names.add("a");
        names.add("b");
        names.add("c");
        names.add("d");
        names.add("e");
        names.add("f");
        Generator generator = new Generator(names);
        ArrayList<ArrayList<String>> groups = generator.createSubgroups();
        String name = "a";
        ArrayList<String> group = generator.findGroup(groups, name);
        assertEquals(group.contains(name), true);
    }

Commit your changes:

    $ git add src/test
    $ git status
    $ git commit -m "define unit tests"

Congrats! You now have experience with three common types of testing. Before wrapping up, we’ll touch briefly on a couple practical niceties: fixtures and git hooks.

## Fixtures

Sometimes we need to use a set of data in several of our tests. For example, the code above instantiates a list of names repeatedly. JUnit defines something called a [test fixture](https://github.com/junit-team/junit/wiki/Test-fixtures), which can help.

Define a class member for the fixture you’d like to provide to your tests:

    public class AppTest {
        private List<String> names;
        …

To populate the list of names before every test, we can “annotate” a function with @Before:

    import org.junit.Before;
    ...
        @Before
        public void setUp() {
            this.names = new ArrayList<String>();
            names.add("a");
            names.add("b");
            names.add("c");
            names.add("d");
            names.add("e");
            names.add("f");
        }

Now we can simplify our tests:

    @Test
    public void groupSize() {
        Generator generator = new Generator(this.names);
        ...
    }

Commit this code to your branch as above

## Git hooks

We can use a [git hook](http://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) to ensure we always run at least a sanity check test before committing our code. A git hook is a script that executes in response to a git events like commit, push, pull, etc. We’ll take a step in the direction of test automation by defining a pre-commit hook to run our tests before each commit.

Building on our Unix experience, copy git’s sample pre-commit file:

    $ cp .git/hooks/pre-commit.sample .git/hooks/pre-commit

Open the file for editing:

    $ vim .git/hooks/pre-commit

Replace the contents with an invocation of our test script:

    #!/bin/bash
    
    if ! mvn test; then
      echo "Failing tests! Aborting commit!"
      exit 1
    fi

The first line tells the terminal to run this file using Bash. Then we have some conditional logic that fails if our test script failed with a non-zero exit code.

Define a breaking change, eg:

    ...
    @Test
    public void groupSize() {
        Generator generator = new Generator(this.names);
        ArrayList<ArrayList<String>> groups = 
          generator.createSubgroups();
        assertEquals(groups.size(), 1);
    ...

Try to commit this change:

    $ git add test.sh
    $ git commit -m “add breaking change”
    [INFO] Scanning for projects...
    ...
    Failed tests:   groupSize(com.mycompany.app.GeneratorTest): expected:<2> but was:<1>
    
    Tests run: 4, Failures: 1, Errors: 0, Skipped: 0
    ...
    Failing tests! Aborting commit!

Observe that the change is still staged and has not been committed:

    $ git status
    $ git log
    
## Conclusion

Our review group generator reduces down to a few steps: shuffling, which we don't need to test because it comes from an external library, creating subgroups, and finding a user in a subgroup. We have unit tests for the last two steps. We have an integration test to assert our units are composed correctly, and a functional test to assert everything prints ok. 

Going forward, we can approach problems in the reverse order: identify the algorithm, write unit tests for each step of the algorithm, write integration tests to assert all the units work well together, and functional test to assert the final product works as intended. When we're working Android, we'll build on our JUnit experience and use UI testing for our functional tests.

We now know our code inside and out and can easily verify it’s still working after making a change. Testing, incremental development and version control are a powerful combination for quality engineering.

We even learned a bit about testability, test fixtures, and test automation.

Please generate your review group for exercise 2, push your branch to your repository, create a pull request and CC your reviewers.

