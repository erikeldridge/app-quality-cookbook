h1. Section 2

Integration testing
Testability
Static analysis


h2. Testable 1
Write
- Exercise: modify the web service to:
-- accept email address or phone number
-- use three maps:
--- email --> user id
--- phone number --> user id
--- user id --> user name
-- use a single fn that:
--- accepts the contact identifier identifier
--- determines the appropriate map to look up the identifier
--- looks up the identifer to resolve the user id
--- returns null or user id
--- looks up the user id to resolve the name
--- returns null or name
- Exercise: write a header comment describing:
-- the purpose of the fn
-- the inputs it takes
-- the output it produces
Test
- Exercise: assert the original test still runs
- Exercise: define a new test for the additional identifier
- Exercise: push the code to review
- Exercise: review code noting:
-- what type of contact identifier was used?
- Exercise: merge code
Reflect
- Writer: was it easy to write the test for this fn?
- Reviewer: was it easy to understand what this fn was doing?
- Accomplishment: experience with writing spaghetti code
- Accomplishment: experience with conflating integration and unit tests


h2. Testable 2
Write
- Exercise: refactor the fn as follows:
-- extract the identifer type resolution
-- extract the identifer lookup into separate fn
-- extract the user lookup into a separate fn
Test
- Exercise: write unit tests for each fn
- Exercise: run unit and integration tests
Reflect
- Writer: was it difficult or easy to modify this fn?
- Writer: was it difficult or easy to write a test for this fn?
- Reviewer: was it more easy or difficult to review these changes?
- Reviewer: are the comments still accurate?
- Accomplishment: experience with SRP


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