# Style analysis

## Goals

* Identify appropriate tooling
* Use style analysis on the command line
* Incorporate style analysis into your IDE

## Motivation

Having an objective style definition can simplify reviews and improve standardization in the code base

Prerequisites:

* IntelliJ, for the IDE portion

## Steps

### Tooling

An appropriate tool should be:
* easily available
* configurable
* usable from development and CI machines

### Using

Create a runner class foo.java:

```
class Runner {
	public static void main(String[] args) {
	}
}
```

Run checkstyle:

```
java -jar checkstyle-6.1.1-all.jar -c google_checks.xml foo.java
```

Observe:

```
warning: The name of the outer type and the file do not match.
```

Review Checkstyle's list of code and implement a change that violates it.

Incorporate checkstyle in your IDE

## Reflect

* Did analysis catch anything?

## Related

* [Checkstyle's Google style config](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml)
* [Google's Java style guide](https://google-styleguide.googlecode.com/svn-history/r130/trunk/javaguide.html)