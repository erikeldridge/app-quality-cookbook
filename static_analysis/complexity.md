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

Create a file Foo.java:
```
// Code from: http://pmd.sourceforge.net/pmd-4.3.0/rules/codesize.html
public class Foo {
    public void fiddle(){}
    public void example(int a, int b, int c, int d, int e, int f, int h, int a1, int a2, int b1, int b2, int z) {
        if (a == b) {
            if (a1 == b1) {
                fiddle();
            } else if (a2 == b2) {
                fiddle();
            }  else {
                fiddle();
            }
        } else if (c == d) {
            while (c == d) {
                fiddle();
            }
        } else if (e == f) {
            for (int n = 0; n < h; n++) {
                fiddle();
            }
        } else{
            switch (z) {
                case 1:
                    fiddle();
                    break;
                case 2:
                    fiddle();
                    break;
                case 3:
                    fiddle();
                    break;
                default:
                        fiddle();
                        break;
                }
            }
    }
}
```

Run pmd:

```
./pmd-bin-5.2.3/bin/run.sh pmd -R java-basic,java-codesize -d Foo.java
```

Observe PMD’s output:

```
$ ./pmd-bin-5.2.3/bin/run.sh pmd -R java-basic,java-codesize -d Foo.java
/home/vagrant/Foo.java:1:	The class 'Foo' has a Cyclomatic Complexity of 6 (Highest = 11).
/home/vagrant/Foo.java:1:	The class 'Foo' has a Standard Cyclomatic Complexity of 6 (Highest = 11).
/home/vagrant/Foo.java:3:	Avoid long parameter lists.
/home/vagrant/Foo.java:3:	The method 'example' has a Cyclomatic Complexity of 11.
/home/vagrant/Foo.java:3:	The method 'example' has a Standard Cyclomatic Complexity of 11.
```

Note: the "ruleset" format is java-<ruleset file name>. For example, pass "java-strings" to use [java/strings.html](http://pmd.sourceforge.net/pmd-5.2.3/pmd-java/rules/java/codesize.html).

### IDE

We can use the [QAPlug's PMD plugin for IntelliJ](http://qaplug.com/about/tutorials/) to integrate PMD.

## Related

* [PMD rulesets](http://pmd.sourceforge.net/pmd-5.2.3/pmd-java/rules/index.html)
