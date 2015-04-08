# Static analysis and continuous integration

The phrase "static analysis" refers to inspecting the aspects of software that do not change, like source code, byte code, etc., for correctness. Code is _static_ in that it doesn't change, as opposed to things like memory and network usage, which are _dynamic_ and change while software is running.

In simplistic terms, we perform static analysis when we read and review code. Our compiler performs static analysis when it parses our code. 

We'll explore four categories of static analysis tools in this section:
* Lint
* Style check
* Bug check
* Test coverage

We'll conclude by introducing continuous integration, which we can use to automatically perform tasks, like running tests and static analysis, in response to changes in a code base.
