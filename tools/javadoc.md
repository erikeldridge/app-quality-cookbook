# Javadoc

Javadoc provides a near-industry-standard framework for programmatically generating documentation.

## Prerequisites

* Java
* Maven

## Verify

See if javadoc is already installed:

```
$ javadoc
javadoc: error - No packages or classes specified.
usage: javadoc [options] [packagenames] [sourcefiles] [@files]
...
```

See if it's alraedy integrated with Maven:

```
$ mvn javadoc:javadoc
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building my-app 1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] >>> maven-javadoc-plugin:2.10.1:javadoc (default-cli) @ my-app >>>
...
```
