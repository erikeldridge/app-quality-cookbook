# Single responsibility principle

## Goals

* Refactor code to express SRP

## Motivation

The single responsibility principle (SRP) is a powerful tool for keeping code clean and easy to understand.

## Prerequisites

* Spaghetti code

## Steps

### Refactor

* extract the identifer type resolution
* extract the identifer lookup into separate fn
* extract the user lookup into a separate fn
* write unit tests for each fn
* run unit and integration tests

## Reflect

* Was it easy to name these functions?
* Was it easy to test these functions?
* Was it easy to review these functions?
* Was it easy to modify these functions?