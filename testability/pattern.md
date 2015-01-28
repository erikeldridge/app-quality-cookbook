# Refactoring to patterns

## Motivation

The culture of design patterns provides us with not only institutional knowledge, but also a shared language we can use to identify issues and approach fixes.


## Goals

Gain experience identifying a code smell, resolving a pattern, and refactoring


## Steps

- Exercise: refactor the fn to:
-- accept a third type of contact identifier, eg username
-- look up the identifier in the appropriate map
-- use factory pattern to alleviate conditional complexity


## Verify

- Exercise: run tests
- Exercise: update tests to cover all three types


## Reflect

- Writer: was it difficult or easy to modify this fn?
- Writer: was it difficult or easy to test this fn?
- Reviewer: are the comments still accurate?
- Note: comment drift
- Accomplishment: experience with self-documenting code
- Accomplishment: experience with refactoring code smell to pattern