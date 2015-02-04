# Version control

## Goals

* Explore repository strategy
* Explore workflow syntax, eg committing, reverting, merging, conflict resolution, tagging

## Motivation

Version control provides us with the ability to create history-aware versions of a code base.

These snapshots are named and immutable, which simplifies reasoning. For example, we can deploy version 123 and know exactly what is included.

The snapshots, aka changes, commits, etc., can also hold context, eg the author, a description of the state being commited, the date the change was made, etc, which facilitates investigation.

Best-practice: only commit working code; the main branch should always be deployable.

## Prerequisites

* Git
* Github

## Steps

### Strategy

Opinion alert!

* Master shippable at all times
* Prefer small commits over monolithic changes
* Do not commit dead code
* Style commit messages according to git guidelines
* Include bug ticket, when available

### Cloning a repo

### Committing a change

### Creating a review

### Approve change review

### Deploying via merging
