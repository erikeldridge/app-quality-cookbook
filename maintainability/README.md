# Maintainability

Think about one fundamental question when writing or reviewing code: How am I going to test this? – [Misko Hevery](http://misko.hevery.com/attachments/Guide-Writing%20Testable%20Code.pdf)

Code that is easier to read is easier to understand. Code that is easier to understand is easier to maintain. Code that is easier to maintain has less bugs. Code that has few bugs is less frightening to deploy. – [@javi](https://twitter.com/javi)

The phrase "maintainability" refers to the ease with which we can maintain our code. Testability and readability are two important aspects of maintainability.

This exercise covers five common aspects of maintainable code:
* avoiding side effects
* injecting dependencies
* communicating intent
* adhering to known patterns
* doing one thing well

We'll build a configuration generator to explore these ideas. We'll start by implementing code with low maintainability, i.e., "spaghetti" code, and then refactor to make it more maintainable.
