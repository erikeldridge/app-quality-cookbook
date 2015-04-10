# Java

Java provides a mature, widely used programming language and runtime.

In terms of quality engineering, many tools are written in Java and/or designed to support it.

## Install

This book’s Vagrantfile should have installed Java for you, but in case that didn’t work [download and install the Java Development Kit (JDK) from Sun](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

Verify installation by checking the version of the Java compiler:

```nohighlight
$ javac -version
javac 1.7.0_75
```

## Build and run

Using Vim, create a file called Main.java:

```nohighlight
$ vim Main.java
```

All Java applications have a [main method](http://docs.oracle.com/javase/tutorial/getStarted/application/index.html) Java calls to kick off the program. Copy/paste the code below into your Main.java:

```java
class Main {
  public static void main(String[] args) {
    System.out.println("Hi");
  }
}
```

Compile using the java compiler:

```nohighlight
$ javac Main.java
```

View the resulting Main.class:

```nohighlight
$ ls
Main.java Main.class
```

Run it:

```nohighlight
$ java Main
Hi
```

Repeat these steps to play with the Java syntax below.

## Variables

Variables let us assign a value to a name:

```java
int number = 0;
```

## Operators

Operators enable us to perform actions on variables:

```java
number = 1; // assignment
System.out.println(number);
number = i + 1; // addition
System.out.println(number);
```

## Loops

Loops enable us to iterate over a collection of values:

```java
String[] values = {"a", "b", "c"};
for (int i = 0; i < values.length; i++) {
  System.out.println(values[i]);
}
```

## Conditions

Conditions allow us to require something to be true:

```java
for (int i = 0; i < 10; i++) {
  if (i % 2 == 0) {
    System.out.println("even");  
  } else {
    System.out.println("odd");  
  }
}
```

## Functions 

Function allow us to name a set of commands:

```java
static int addOne(int i) {
  return i + 1;
}
… 
System.out.println(addOne(1));
```

## Classes

Classes enable us to encapsulate common functionality:

```java
class Number {
  int i;
  Number(int i) {
    this.i = i;
  }
  void add(int i) {
    this.i += 1;
  }
  boolean isEven() {
    if (i % 2 == 0) {
      return true;
    } else {
      return false;
    }
  }
}
… 
class Hi {
    public static void main(String[] args) {
      Number n = new Number(1);
      System.out.println(n.isEven();
      System.out.println(n.add(1));
      System.out.println(n.isEven());
…
```

## Imports

Imports let us include encapsulated code:

```java
import java.util.ArrayList
… 
class Main {
    public static void main(String[] args) {
      ArrayList<String> emails = new ArrayList<String>();
      emails.add("a@csumb.edu");
      System.out.println(emails.toString());
```

## Exercise

Write a program that accepts two numeric arguments and uses the Number class to determine if the sum of the numbers is even:

```nohighlight
$ java Main 1 3
true
$ java Main 10 5
false
```

## Learn more

We're in the midst of a golden age for [languages based on the Java Virtual Machine (JVM)](http://en.wikipedia.org/wiki/List_of_JVM_languages), an extremely mature piece of software. Take a look at [Scala](http://www.scala-lang.org/), [Clojure](http://clojure.org/), [Kotlin](http://kotlinlang.org/), and [Groovy](http://groovy-lang.org/)