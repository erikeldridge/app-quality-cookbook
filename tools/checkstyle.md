# Checkstyle

## Motivation

We can use style analysis tools to define an objective standard for code syntax.

Having a standard style can make code easier to read and avoid subjective debates.

Checkstyle is effective, widely used, and can be run in an IDE and CI.


## Goals

* Install checkstyle
* Use checkstyle to analyze a piece of code


## Steps

1. Download [checkstyle](http://checkstyle.sourceforge.net/)
1. Download [Google’s style config](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml)


## Verify

1. Run checkstyle on the hi.java file created in the [java tools introduction](tools/java.md):
```
java -jar checkstyle-6.1.1-all.jar -c google_checks.xml hi.java
```
2. Observe:
```
warning: The name of the outer type and the file do not match.
```