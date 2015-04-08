# Integration testing

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

## A quick dive into Maven

Maven simplifies and standardizes build configuration. It is commonly used, and other tools like Gradle can pull from Maven repositories.

### Install

    $ sudo apt-get install maven
    ...
    $ mvn -version
    Apache Maven 3.0.5
    Maven home: /usr/share/maven
    Java version: 1.7.0_75, vendor: Oracle Corporation
    Java home: /usr/lib/jvm/java-7-openjdk-i386/jre
    Default locale: en_US, platform encoding: UTF-8
    OS name: "linux", version: "3.13.0-45-generic", arch: "i386", family: "unix"

### Create quickstart project

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

### Create Maven-based IntelliJ project

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

## Write integration tests

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