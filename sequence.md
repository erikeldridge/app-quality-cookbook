# A suggested reading sequence

In the spirit of iteration, I recommend introducing core concepts and then building up.

## 1

1. [preface](preface.md) tldr and goals
1. Drawing exercises 1, 2, and 3

Concepts:

* How do you know if the shape is drawn correctly?
* If we don't get feedback, how do we know if we've drawn a shape correctly?
* Was it easier to draw the shape correctly with feedback?

## 2

1. [preface](preface.md) bias toward scope over quality wrt solo development
1. [Unix intro](tools/unix.md)
1. [Unix fs and ssh exercise](exercises/unix.md)
1. Text editing intro with [Vim](tools/vim.md)
1. Writing code with [Java](tools/java.md)
1. [Java exercise 1](exercises/java.md)
1. [Static analysis intro](static_analysis/README.md) with [Lint](static_analysis/lint.md)
1. JDB intro
1. [Testing theory 1](testing/README.md)
1. [Version control](version_control/README.md) with [Git](tools/git.md)
1. [Java exercise 2](exercises/java.md)

Concepts:
* How efficiently can we write code with a simple text editor?
* How confident are we in code written in a text editor?
* Does running code through a compiler help improve confidence?
* Does the compiler help identify problems?
* How do you know if our code is working correctly?
* How do you fix issues as they are discovered?

## 3

1. Maven intro
1. [IntelliJ intro](tools/intellij)
1. [Review group exercise](exercises/grouper.md)
1. [Code review intro](static_analysis/code_review.md)
1. Hosted respository intro with [Github](tools/github.md)
1. Bug report exercise
1. [Unit testing intro](testing/unit.md) with [JUnit](tools/junit.md)
1. [Integration testing intro](testing/integration.md)
1. Pull request exercise

Concepts:
* Was it easier to write code in an IDE vs vim?
* Do you feel more confident with code written in an IDE?
* Did you have less compiler errors from code written in an IDE?
* Did you feel more confident after tests were added?
* Do you feel more confident about changes made while pairing?

## 4

1. Communication
1. Shared principles
1. [Pairing intro](collaboration/pairing.md)
1. Testability intro
1. Spaghetti code exercise
1. Static analysis intro
1. Style analysis with Checkstyle
1. Complexity analysis intro with PMD
1. Code coverage intro with JaCoCo
1. Single responsibility intro
1. Single responsibility exercise
1. Dependency injection intro with Dagger 2
1. Dependency injection exercise
1. Refactoring to patterns intro
1. Refactoring to patterns exercise
1. Communicating intent intro
1. Communicating intent exercise
1. Documentation intro with Javadoc

Concepts:
* Did you find testability patterns like dependency injection made code easier to reason about and test?
* Did documentation make code easier to understand?
* Does static analysis help you feel more confident about the code base?

## 5

1. [Automation intro](automation/README.md)
1. Scripting intro with Bash
1. Pre-push hook exercise
1. Web service intro with Jetty
1. REST intro with Jersey
1. Fractional config web service exercise
1. Functional test intro
1. Build config intro
1. Immutable platform intro
1. Heroku intro
1. Monitoring intro
1. New relic intro
1. Monitoring exercise
1. Log analysis intro
1. Log analysis exercise

## 6

1. Continuous integration intro
1. Jenkins intro
1. Deployment automation
1. Staged deployment
1. Dynamic routing
1. Change management
1. Alerting
1. Oncall
1. Configuration-based damage control
1. Failover
1. Rolling back
1. Root cause
1. Code search
1. Postmortem
1. Practice
1. Redline testing

## 7

1. Pairing
1. Android
1. Android Studio
1. Android unit testing
1. Robolectric
1. Test-driven development
1. Native feature preference
1. Build configuration
1. Dynamic configuration
1. Design by contract
1. API integration
1. Versioning intro
1. Backwards compatibility
1. Graceful degradation

## 8

1. UI testing
1. Tracing
1. Crash monitoring
1. Continuous integration for Android
1. Staged deploy for Android
1. Performance monitoring
1. Product usage monitoring
1. Resource usage monitoring
1. Fractional deployment



