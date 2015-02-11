# Unit testing

When functionality becomes complex, it can be difficult to determine if it is working correctly. Unit tests can help assert the correctness of the units of code comprising a complex system.

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

Create a review for the review group generator exercise.

Add the reviewers in your review group to this review.

After reviewing, use JUnit to write unit tests for this generator, and then update your review.

## Reflect

* Philosophy: small units
* Philosophy: quick to run
* Was the code easier or more difficult to read with the tests?

## Related

* [Martin Fowler's article on unit testing](http://martinfowler.com/bliki/UnitTest.html)