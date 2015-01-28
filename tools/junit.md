# JUnit

## Motivation

Tests for small, single-purpose pieces, aka units, of code are called unite tests.

JUnit is a widely used framework for unit testing Java.

Experience with JUnit can be applied to many other testing frameworks.

## Goals

* Install JUnit
* Gain experience defining and running a test


## Steps

1. [Download the jars](https://github.com/junit-team/junit/wiki/Download-and-Install)


## Verify

1. Create a file called HiTest.java
1. Define a [passing test](https://github.com/junit-team/junit/wiki/Getting-started)
1. Compile the test:
```
javac -cp "./*" HiTest.java
```
1. Run the test:
```
java -cp ".:./*" org.junit.runner.JUnitCore HiTest
```
1. Observe output:
```
JUnit version 4.12-beta-3
Time: 0.008
OK (1 test)
```