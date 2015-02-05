# Complexity analysis

## Goals

* Identify appropriate tooling
* Command line usage
* IDE integration

## Motivation

We can use complexity analysis tools to objectively measure flaws in our code.

This objective analysis can make code review more efficient by weeding out problems before they reach review, and catching issues uniformly.

## Prerequisites

* Java

## Steps

### Tooling

An appropriate tool should:
* be configurable
* generate machine-readable output for CI
* ideally, it will use a common descriptions to pair the code smells with appropriate solutions.

### Command line

Add an unused variable to Main.java created in the [Java installation](tools/java_installation.md) section

```
class Main {
    public static void main(String[] args) {
        String unused = "unused";
    }
}
```

Run pmd:

```
./pmd-bin-5.2.2/bin/run.sh pmd -R java-basic,java-unusedcode -d ./
```

Observe PMD’s output:

```
Avoid unused local variables such as 'unused'.
```

### IDE

IntelliJ has a [plugin for PMD](http://pmd.sourceforge.net/pmd-4.3.0/integrations.html#idea).
