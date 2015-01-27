# JUnit

We’ll use junit for unit testing. 

## Install

1. download the jars

## Verify

2. create a file called HiTest.java
3. define a passing test
4. compile the test:
```
javac -cp "./*" HiTest.java
```
5. run the test:
```
java -cp ".:./*" org.junit.runner.JUnitCore HiTest
```
6. observe output:
```
JUnit version 4.12-beta-3
Time: 0.008
OK (1 test)
```