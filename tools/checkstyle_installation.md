# Checkstyle

We’ll use checkstyle to define an objective standard for code syntax. 

## Install

1. Download [checkstyle](http://checkstyle.sourceforge.net/)
2. Download [Google’s style config](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml)

## Verify

1. Run checkstyle on the hi.java file created in the [java tools introduction](tools/java.md):
```
java -jar checkstyle-6.1.1-all.jar -c google_checks.xml hi.java
```
2. Observe:
```
warning: The name of the outer type and the file do not match.
```