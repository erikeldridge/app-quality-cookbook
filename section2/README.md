h1. Section 2

Integration testing
Testability
Static analysis





h2. Testable 3
Write
- Exercise: refactor the service's request class to accept the lookup maps as arguments
Test
- Exercise: update and run tests
- Exercise: push to review
Reflect
- Writer: did this change make it more easy or difficult to test?
- Accomplishment: experience with DI


h2. Testable 4
Write
- Exercise: refactor the fn to:
-- accept a third type of contact identifier, eg username
-- look up the identifier in the appropriate map
-- use factory pattern to alleviate conditional complexity
Test
- Exercise: run tests
- Exercise: update tests to cover all three types
Reflect
- Writer: was it difficult or easy to modify this fn?
- Writer: was it difficult or easy to test this fn?
- Reviewer: are the comments still accurate?
- Note: comment drift
- Accomplishment: experience with self-documenting code
- Accomplishment: experience with refactoring code smell to pattern


h2. Testable 5
Write
- Exercise: refactor the code to fn and variable names instead of comments to communicate the intent
Test
- Exercise: run tests
- Exercise: push to review
Reflect
- Reviewer: review code noting if you can still tell what the code does
- Accomplishment: experience with self-documenting code
- Philosophy: simple code is easier to review; reviewed code is easier to maintain; maintained code has fewer bugs; code w/ fewer bugs is less frightening to ship // credit: Javi


h2. Static analysis 2
Write
- Lesson: PMD
- Exercise: check out the commit from Testable 1
Test
- Exercise: run PMD
Reflect
- Philosophy: compilation and self- and team-review are forms of static analysis
- Did analysis catch anything?
- If so, what? // this should overlap w/ the refactors made above
- Accomplishment: experience with programmatic complexity analysis
- Accomplishment: experience w/ git history