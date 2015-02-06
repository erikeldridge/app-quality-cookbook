# Spaghetti code

## Goals

* Write spaghetti code

## Motivation

Some code is unpleasant to work with because it's dependencies and influences are poorly defined. Because it is difficult to understand, it is difficult to modify without side effects. A common term for such code is spaghetti.

## Prerequisites

* Integration testing

## Steps

### Writing spaghetti code

Modify doGet from our integration testing as follows:
* defines three maps:
	* email --> user id
	* phone number --> user id
	* user id --> user name
* accepts a contact identifier
* determines the appropriate map to look up the identifier
* looks up the identifier to resolve the user id
* returns null or user id
* looks up the user id to resolve the name
* returns an empty string or the name

### Describe

Write a header comment describing:
* the purpose of the controller
* the inputs it takes
* the output it produces

### Test

Write a test for this controller

### Review

## Reflect

* Was it easy to name this function?
* Was it easy to test this function?
* Was it easy to review this function?
* Was it easy to modify this function?
