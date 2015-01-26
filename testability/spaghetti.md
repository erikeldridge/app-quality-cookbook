# Testability: spaghetti

## Write

Exercise: modify the web service to:
- accept email address or phone number
- use three maps:
    - email --> user id
    - phone number --> user id
    - user id --> user name
- use a single fn that:
    - accepts the contact identifier identifier
    - determines the appropriate map to look up the identifier
    - looks up the identifer to resolve the user id
    - returns null or user id
    - looks up the user id to resolve the name
    - returns null or name

Exercise: write a header comment describing:
- the purpose of the fn
- the inputs it takes
- the output it produces

## Test

- Exercise: assert the original test still runs
- Exercise: define a new test for the additional identifier
- Exercise: push the code to review
- Exercise: review code noting:
-- what type of contact identifier was used?
- Exercise: merge code

## Reflect

- Writer: was it easy to write the test for this fn?
- Reviewer: was it easy to understand what this fn was doing?
- Accomplishment: experience with writing spaghetti code
- Accomplishment: experience with conflating integration and unit tests