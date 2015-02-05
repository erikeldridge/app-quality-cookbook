# Java

## Goals

* Install
* Dive into Java syntax

## Motivation

Java provides a mature, widely used programming language and runtime

In terms of quality engineering, many tools are written in Java and/or are designed to support it.

## Prerequisites

* Vagrant

## Steps

### Install

The [virtual machine associated with this book](tools/vagrant.md) will manage installation. Please refer to the [machine's configuration](../Vagrantfile) for more details.

### Diving in

Create a file called Main.java

Put hello world into it:

```
    class Main {
        public static void main(String[] args) {
            System.out.println("hi");
        }
    }
```

Compile: 

```
$ javac Main.java
```

Run:

```
$ java Main
```

Observe “hi” printed to the console

## Reflect

* With respect to quality engineering, how do you know if the code you wrote was correct?