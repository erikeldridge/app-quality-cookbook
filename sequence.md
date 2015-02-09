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
1. [Unix fs and ssh exercise](](exercises/unix.md))
1. [Vim intro](tools/vim.md)
1. [Vim exercise](tools/vim.md)
1. [Java intro](tools/java.md)
1. [Java exercise 1](exercises/java.md)
1. [Static analysis intro](static_analysis/README.md)
1. [Lint intro](static_analysis/lint.md)
1. JDB intro
1. [Testing theory 1](testing/README.md)
1. [Version control intro](version_control/README.md)
1. [Git intro](tools/git.md)
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
1. Over-the-shoulder code review
1. Github intro
1. Bug report exercise
1. IntelliJ debugger
1. Unit testing intro
1. Integration testing intro
1. JUnit intro
1. JUnit exercise
1. Bug resolution exercise
1. Pull request exercise
1. Pairing exercise
1. Javadoc intro

Concepts:
* Was it easier to write code in an IDE vs vim?
* Do you feel more confident with code written in an IDE?
* Did you have less compiler errors from code written in an IDE?
* Did you feel more confident after tests were added?
* Do you feel more confident about changes made while pairing?

## 4

1. Testability intro
1. Spaghetti code exercise
1. Static analysis intro
1. Checkstyle intro
1. PMD intro
1. JaCoCo intro
1. Single responsibility intro
1. Single responsibility exercise
1. Dependency injection intro
1. Dependency injection exercise
1. Refactoring to patterns intro
1. Refactoring to patterns exercise
1. Communicating intent intro
1. Communicating intent exercise
1. Documentation intro
1. Javadoc intro
1. Fractional config exercise
1. Code review

Concepts:
* Did you find testability patterns like dependency injection made code easier to reason about and test?
* Did documentation make code easier to understand?
* Does static analysis help you feel more confident about the code base?

## 5

Automation
bash scripting
exercise: run tests and static analysis as a pre-push hook
Deployment
staged (local, remote)
note: more on staged rollout (canary, alpha, beta) in next session
note: immutable platform, consistent environments
exercise: heroku intro ← note: will need to install heroku & maven
exercise: web app wrapping fractional config
exercise: web app tests
Monitoring
does it work?
Exercise: new relic
Exercise: configure app to use new relic
Configuration
Exercise: build config
dynamic config
Damage control
rollback
exercise: adjust fractional availability up via build config
exercise: adapt app to ingest dynamic config
exercise: adjust fractional availability up via dynamic config
Testing
Integration
Backwards compatibility
intro
exercise: refactor config endpoint from / to /1/config






