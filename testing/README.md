# Testing

More than the act of testing, the act of designing tests is one of the best bug preventers known. The thinking that must be done to create a useful test can discover and eliminate bugs before they are coded. – Boris Beizer

In this exercise, we’ll explore three commonly used, broad categories of testing:
* unit
* integration
* functional

Unit tests assert correct behavior for small “units” of code.

Integration tests assert correct behavior for a composition of units.

Functional tests assert correct behavior of an entire product.

As Martin Fowler describes in his [Test Pyramid](http://martinfowler.com/bliki/TestPyramid.html) article, we should build on a base of unit tests, as these are the easiest to reason about and the fastest to run.

We’ll work with the code review group generator we created earlier. In the context of this exercise, a unit could be a function that splits our team into groups of three developers; an integration test could cover assignment of a person to a group; a functional test could verify the printed output is correctly formatted.

Since we wrote our code before writing the tests, we’ll apply tests in reverse order: functional, integration, units. This is actually a best-practice: when working with code that may need to be “refactored”, i.e., changed structurally without affecting the output, write tests first to help verify the program works the same before and after the change.