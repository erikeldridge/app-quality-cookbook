# Flow

## Goals

* Explore approaches to testing

## Motivation

Tests can help build confidence, but they can be a pain if they are used inappropriately

## Steps

### Ordered by cost

* Unit test before commit
* Integration tests before pushing
* Device testing in CI

### Automated for reliability

* Unit & integration test before merging, enforced by code review tooling
* Device test periodically
* Unit, integration, and device testing before release

### Best-practices

* Fix rather than skip tests
* Define tests before refactoring
* Prioritize tests for complex, and high-traffic and -value, code