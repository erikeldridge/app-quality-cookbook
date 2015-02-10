# JUnit

JUnit is a widely used framework for unit testing Java.

Experience with JUnit can be applied to many other testing frameworks.

## Prerequisites

* Java
* Maven

## Install

Install and run via Maven. Edit pom.xml to include:

```
<dependencies>
	<dependency>
	  <groupId>junit</groupId>
	  <artifactId>junit</artifactId>
	  <version>4.10</version>
	  <scope>test</scope>
	</dependency>
</dependencies>
```

## Verify

```
$ mvn test
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building my-app 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
...
[INFO] 
[INFO] --- maven-resources-plugin:2.3:testResources (default-testResources) @ my-app ---
[WARNING] Using platform encoding (UTF-8 actually) to copy filtered resources, i.e. build is platform dependent!
[INFO] skip non existing resourceDirectory /home/vagrant/my-app/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:2.0.2:testCompile (default-testCompile) @ my-app ---
[INFO] Compiling 1 source file to /home/vagrant/my-app/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:2.10:test (default-test) @ my-app ---
[INFO] Surefire report directory: /home/vagrant/my-app/target/surefire-reports

-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.mycompany.app.AppTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.036 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.895s
[INFO] Finished at: Tue Feb 10 07:35:24 UTC 2015
[INFO] Final Memory: 13M/32M
[INFO] ------------------------------------------------------------------------
```

## Related

* [JUnit's getting started docs](https://github.com/junit-team/junit/wiki/Getting-started)


