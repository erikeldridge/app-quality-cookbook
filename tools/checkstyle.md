# Checkstyle

We’ll use checkstyle to define an objective standard for code syntax. 

## Install

1. download checkstyle
2. download Google’s style config

## Verify

Run it on the hi.java file created above:
1. java -jar checkstyle-6.1.1-all.jar -c google_checks.xml hi.java
2. observe checkstyle’s output:
warning: The name of the outer type and the file do not match.