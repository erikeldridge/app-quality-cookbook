# Code coverage

Code coverage analysis can tell us how many lines of our code are touched by our tests.

Having 100% coverage doesn't guarantee freedom from bugs, in part because our tests don't cover all usage patterns, but it can help us identify under-maintained portions of our code.

## Prerequisites

* JaCoCo
* Maven

## Steps

### Tooling

An appropriate tool should be runnable from the command line and produce machine-readable output for CI

### Using JaCoCo on the command line

Run:
```
$ mvn verify
...
[INFO] --- jacoco-maven-plugin:0.7.2.201409121644:check (default-check) @ foo ---
[INFO] Analyzed bundle 'foo' with 1 classes
[WARNING] Rule violated for bundle foo: complexity covered ratio is 0.00, but expected minimum is 0.60
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
...
```

### Using JaCoCo in IntelliJ

Create Maven project and run verify

## Related

* [IntelliJ's Code Coverage documentation](https://www.jetbrains.com/idea/help/code-coverage.html)
* [JaCoCo's maven plugin documentation](http://www.eclemma.org/jacoco/trunk/doc/maven.html)

