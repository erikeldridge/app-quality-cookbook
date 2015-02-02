# Unit testing

## Motivation

When functionality becomes complex, it can be difficult to determine if it is working correctly. Unit tests can help assert the correctness of the units of code comprising a complex system.

## Goals

* Identify appropriate unit tests
* Gain experience writing and reading unit tests

## Prerequisites

* Java

## Steps

### Appropriate unit tests

Tests for small, single-purpose pieces, aka units, of code are called unit tests.

To paraphrase Martin Fowler:
* low-level, focusing on a small part of the software system. 
* significantly faster than other kinds of tests

Avoid testing imported code; each project is responsible for its own tests

### Write a unit test

We need something of reasonable complexity to test, so use Java to create a utility for generating a "review group" composed of you and two random reviewers.

This utility should accept the following inputs:
* a comma-separate sequence of all reviewers, including yourself, as a string
* your username as a string
* an integer exercise identifier

The utility should return a review group as a comma-separated sequence of three usernames:

```
$ java Grouper a,b,c,d,e,f e 1
e,c,f
```

Each member of your group should generate the same sequence. For example, user "f" from the example above would see:

```
$ java Grouper a,b,c,d,e,f f 1
e,c,f
```

Create a review for the generator described above.

Add the reviewers in your review group to this review.

After reviewing, use JUnit to write unit tests for this generator, and then update your review.

## Reflect

* Philosophy: small units
* Philosophy: quick to run
* Was the code easier or more difficult to read with the tests?

## Related

* [Martin Fowler's article on unit testing](http://martinfowler.com/bliki/UnitTest.html)