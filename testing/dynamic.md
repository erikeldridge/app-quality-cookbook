# Dynamic routing

## Goals

* Simplify integration testing by specifying underlying service locations at request time

## Motivation

Rather than require all services to be deployed in the same env, "compose" services by dynamically routing between environments.

## Related

* [Finagle's Wily routing](http://iwishtoinform.blogspot.com/2014/09/finagle-promises-local-and-dtab.html) gets full credit for this idea