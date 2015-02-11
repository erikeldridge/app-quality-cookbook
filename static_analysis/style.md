# Style analysis

## Goals

* Identify appropriate tooling
* Use style analysis on the command line
* Incorporate style analysis into your IDE

## Motivation

We can use style analysis tools to define an objective standard for code syntax.

Having a standard style can make code easier to read and avoid subjective debates.

Prerequisites:

* IntelliJ
* Maven

## Steps

### Tooling

An appropriate tool should be:
* easily available, so everyone can use it
* easy to add and disable rules to
* usable from development and CI machines

### Using checkstyle from the command line

Create a Main class foo.java:

```
import java.util.HashMap;
import java.util.ArrayList;

public class Foo {  
  public Foo(int i){
    this.i = i;
  }
  private int i;
  public Foo(){
    this(0);
  }
}
```

Run checkstyle:

```
$ mvn validate
```

Observe:

```
Starting audit...
/home/vagrant/Foo.java:2: warning: Wrong lexicographical order for 'java.util.ArrayList' import.
/home/vagrant/Foo.java:5:18: warning: Parameter name 'i' must match pattern '^[a-z][a-z0-9][a-zA-Z0-9]*$'.
/home/vagrant/Foo.java:5:20: warning: WhitespaceAround: '{' is not preceded with whitespace.
/home/vagrant/Foo.java:8: warning: 'VARIABLE_DEF' should be separated from previous statement.
/home/vagrant/Foo.java:8:15: warning: Member name 'i' must match pattern '^[a-z][a-z0-9][a-zA-Z0-9]*$'.
/home/vagrant/Foo.java:9: warning: 'CTOR_DEF' should be separated from previous statement.
/home/vagrant/Foo.java:9:15: warning: WhitespaceAround: '{' is not preceded with whitespace.
Audit done.

```

### Using checkstyle in IntelliJ

1. Create a Maven project in IntelliJ
2. Compile
3. Observe checkstyle output

## Related

* [Checkstyle's Google style config](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml)
* [Google's Java style guide](https://google-styleguide.googlecode.com/svn-history/r130/trunk/javaguide.html)
* [Maven checkstyle plugin documentation](http://maven.apache.org/plugins/maven-checkstyle-plugin/usage.html)
* [Android checkstyle config](https://gist.github.com/shareme/4197561)
