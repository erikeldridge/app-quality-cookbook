# Change management

## Motivation

A change management (CM) system helps a team know what is changing, coordinate changes, and determine what's changed recently if there's an issue.

## Goals

* Define a CM system
* Coordinate changes via CM
* Use CM when investigating a bug

## Steps

### Defining CM

CM should have the following qualities:
* broadcast messaging
* searchable logs
* global lock

People making changes should be able to announce changes easily, and oncall engineers should be able to subscribe to these announcements.

Announcements should be collected somewhere accessible so we can search them in case there is a bug.

We should be able to prohibit changes in case of an ongoing bug or time of risk, eg don't deploy the site during peak traffic.

Some simple examples:
* email a list before making a change; reply if the change shouldn't be made
* create a ticket before making a change; block the ticket if the change shouldn't be made
* create a web app that allows us to request a global lock and logs all previous locks

### Coordinating

Before making a change, consult CM. If the lock is available, request it. After making the change, release the lock.

### Using the change log

When investigating an issue, consult the CM log to see what's change recently in the area affected by the issue.
