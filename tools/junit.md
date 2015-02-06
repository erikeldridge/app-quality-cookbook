# JUnit

## Goals

* Install JUnit
* Dive into JUnit syntax

## Motivation

JUnit is a widely used framework for unit testing Java.

Experience with JUnit can be applied to many other testing frameworks.

## Prerequisites

* Vagrant

## Steps

### Install

The [virtual machine associated with this book](tools/vagrant.md) will manage installation. Please refer to the [machine's configuration](../Vagrantfile) for more details.

### Creating tests

Create a file called FooTest.java

```
import static org.junit.Assert.assertEquals;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.junit.runners.JUnit4;

public class FooTest {
    @Test
    public void thisAlwaysPasses() {
        assertEquals(true, true);
    }
}
```

### Compiling tests

```
$ javac -cp junit-4.12.jar FooTest.java
```

### Running tests

```
$ java -cp .:junit-4.12.jar:hamcrest-core-1.3.jar org.junit.runner.JUnitCore FooTest
JUnit version 4.12-beta-3
Time: 0.008
OK (1 test)
```

## Related

* [JUnit's getting started docs](https://github.com/junit-team/junit/wiki/Getting-started)


