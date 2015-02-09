# Java

Java provides a mature, widely used programming language and runtime

In terms of quality engineering, many tools are written in Java and/or are designed to support it.

## Diving in

### Hello world

Create a file called Main.java

```
    class Main {
        public static void main(String[] args) {
            System.out.println("hi");
        }
    }
```

Compile: 

```
$ javac Main.java
```

Run:

```
$ java Main
```

Observe “hi” printed to the console

### Variables

Variables let us assign a value to a name:

```
int number = 0;
```

### Operators

Operators enable us to perform actions on variables:

```
number = 1; // assignment
System.out.println(number);
number = i + 1; // addition
System.out.println(number);
```

### Loops

Loops enable us to iterate over a collection of values:

```
for (int i = 0; i < 10; i++) {
  System.out.println(i);
}
```

### Conditions

Conditions allow us to require something to be true:

```
for (int i = 0; i < 10; i++) {
  if (i % 2 == 0) {
    System.out.println("even");  
  } else {
    System.out.println("odd");  
  }
}
```

### Functions 

Function allow us to name a set of commands:

```
static int addOne(int i) {
  return i + 1;
}
… 
System.out.println(addOne(1));
```

### Classes

Classes enable us to encapsulate common functionality:

```
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

### Imports

Imports let us include encapsulated code:

```
import java.util.ArrayList
… 
class Main {
    public static void main(String[] args) {
      ArrayList<String> emails = new ArrayList<String>();
      emails.add("a@csumb.edu");
      System.out.println(emails.toString());
```

## Learn more

* [Oracle's "Hello World" Java Tutorial](http://docs.oracle.com/javase/tutorial/getStarted/cupojava/unix.html)
* [Oracle's Language Basics Java Tutorial](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/index.html)