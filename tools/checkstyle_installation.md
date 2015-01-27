# Checkstyle

We’ll use checkstyle to define an objective standard for code syntax. 

## Install

1. download checkstyle
2. download Google’s style config

## Verify

1. Run checkstyle on the hi.java file created in the [java tools introduction](tools/java.md):
```
java -jar checkstyle-6.1.1-all.jar -c google_checks.xml hi.java
```
2. observe:
```
warning: The name of the outer type and the file do not match.
```