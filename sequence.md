# A suggested reading sequence

In the spirit of iteration, I recommend introducing core concepts and then building up.

## 1

1. [preface](preface.md) tldr and goals
1. [Drawing exercises](exercises/drawing.md)

## 2

1. [preface](preface.md) bias toward scope over quality wrt solo development
1. [Unix](tools/unix.md)
1. [Vim](tools/vim.md)
1. [Java](tools/java.md)
1. [Static analysis](static_analysis/README.md)
1. [Lint](static_analysis/lint.md)
1. [Debugger](damage_control/debugger.md)
1. [Testing](testing/README.md)
1. [Version control](versioning/version_control.md)
1. [Git](tools/git.md)
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
1. [IntelliJ](tools/intellij)
1. [Review group exercise](exercises/grouper.md)
1. [Code review](static_analysis/code_review.md)
1. Hosted repositories
1. [Github](tools/github.md)
1. Bug report exercise
1. [Unit testing](testing/unit.md)
1. [JUnit](tools/junit.md)
1. [Integration testing](testing/integration.md)
1. Pull request exercise

Concepts:
* Was it easier to write code in an IDE vs vim?
* Do you feel more confident with code written in an IDE?
* Did you have less compiler errors from code written in an IDE?
* Did you feel more confident after tests were added?
* Do you feel more confident about changes made while pairing?

## 4

1. [Communication](collaboration/communication.md)
1. [Shared principles](collaboration/principles.md)
1. [Pairing](collaboration/pairing.md)
1. [Testability](testability/README.md)
1. [Spaghetti code](testability/spaghetti.md)
1. [Static analysis](static_analysis/README.md)
1. [Style analysis](static_analysis/style.md)
1. [Checkstyle](tools/checkstyle.md)
1. [Complexity analysis](static_analysis/complecity.md)
1. [PMD](tools/pmd.md)
1. [Code coverage](static_analysis/coverage.md)
1. [JaCoCo](tools/jacoco.md)
1. [Single responsibility](testability/srp.md)
1. [Dependency injection](testability/di.md)
1. Dagger 2
1. [Refactoring to patterns](testability/pattern.md)
1. [Communicating intent](testability/intent.md)
1. [Documentation](collaboration/documentation.md)
1. [Javadoc](tools/javadoc.md)

Concepts:
* Did you find testability patterns like dependency injection made code easier to reason about and test?
* Did documentation make code easier to understand?
* Does static analysis help you feel more confident about the code base?

## 5

1. [Automation intro](automation/README.md)
1. Command line scripting
1. Bash
1. Pre-push hook exercise
1. Jetty
1. Jersey
1. [Fractional config web service exercise](exercises/service.md)
1. Functional testing
1. [Build configuration](configuration/build.md)
1. Immutable platforms
1. [Heroku](tools/heroku.md)
1. [Monitoring](monitoring/alerting.md)
1. [Alerting](monitoring/alerting.md)
1. [New relic](tools/newrelic.md)
1. Monitoring exercise
1. Log analysis
1. Log analysis exercise

Concepts:
* Did running tests automatically catch any issues?
* Did functional tests catch anything missed by unit or integration tests?

## 6

1. [Continuous integration](automation/ci.md)
1. [Jenkins](jenkins.md)
1. [Deployment automation](automation/deployment.md)
1. [Staged deployment](deployment/staged.md)
1. [Dynamic routing](testing/dynamic.md)
1. [Change management](monitoring/change_management.md)
1. [Oncall](monitoring/oncall.md)
1. [Configuration-based damage control](damage_control/configure.md)
1. [Failover](damage_control/failover.md)
1. [Rolling back](damage_control/rollback.md)
1. [Root cause](damage_control/root_cause.md)
1. [Code search](damage_control/code_search.md)
1. [Postmortem](damage_control/postmortem.md)
1. [Practice](damage_control/practice.md)
1. [Redline testing](testing/redline.md)

## 7

1. Android
1. [Android Studio](tools/android_studio.md)
1. Robolectric
1. [Test-driven development](testing/tdd.md)
1. [Native feature preference](testability/native.md)
1. [UI testing](testing/ui.md)
1. [Dynamic configuration](configuration/dynamic.md)
1. Design by contract
1. API integration
1. [Versioning strategy](versioning/strategy.md)
1. [Backwards compatibility](versioning/compatibility.md)
1. [Graceful degradation](damage_control/degradation.md)
1. Fractional availability exercise

## 8

1. [Tracing](damage_control/tracing.md)
1. [Crash monitoring](monitoring/crash.md)
1. [Device testing](testing/device.md)
1. [Deterministic tests](testing/deterministic.md)
1. [Network testing](testing/network.md)
1. Continuous integration for Android
1. Staged deploy for Android
1. [Performance monitoring](monitoring/performance.md)
1. [Product usage monitoring](monitoring/product_usage.md)
1. [Resource usage monitoring](monitoring/resource_usage.md)




