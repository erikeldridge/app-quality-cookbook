# Dynamic routing

Rather than require all services to be deployed in the same env, "compose" services by dynamically routing between environments.

Dynamic routing simplifies integration testing as we do not need to stage all services required to run the one service being changed.

## Steps

1. Identify route identifier
1. Identify transmission method
1. Modify each topological layer to honor and forward identifier

## Related

* [Finagle's Dtab routing](http://iwishtoinform.blogspot.com/2014/09/finagle-promises-local-and-dtab.html) is an example