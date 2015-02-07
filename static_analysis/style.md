# Style analysis

## Goals

* Identify appropriate tooling
* Use style analysis on the command line
* Incorporate style analysis into your IDE

## Motivation

Having an objective style definition can simplify reviews and improve standardization in the code base

Prerequisites:

* IntelliJ, for the IDE portion

## Steps

### Tooling

An appropriate tool should be:
* easily available
* configurable
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
java -jar checkstyle-6.3-all.jar -c google_checks.xml Foo.java
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

### Using checkstyle in your IDE

We can use the [QAPlug's Checkstyle plugin for IntelliJ](http://qaplug.com/download/) to integrate PMD.

Run the plugin via Tools > QAPlug > Analyze code

Configure via File > Settings ... > Other Settings > QAPlug

## Related

* [Checkstyle's Google style config](https://github.com/checkstyle/checkstyle/blob/master/src/main/resources/google_checks.xml)
* [Google's Java style guide](https://google-styleguide.googlecode.com/svn-history/r130/trunk/javaguide.html)