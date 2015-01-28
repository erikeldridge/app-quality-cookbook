# Java syntax

## Motivation

Having experience with Java will enable us to explore the concepts of quality engineering


## Goal

Gain experience with basic Java syntax: variables, operators, loops, conditions, functions, classes, and imports.


## Steps

Modify your hi.java file to explore anything you're unfamiliar with below.

_Variables_ let us assign a value to a name:
```
int i;
```

_Operators_ enable us to perform actions on variables:
```
i = 1; // assignment
System.out.println(i);
i = i + 1; // addition
System.out.println(i);
```

_Loops_ enable us to iterate over a collection of values:
```
for (int i = 0; i < 10; i++) {
  System.out.println(i);
}
```

_Conditions_
```
for (int i = 0; i < 10; i++) {
  if (i % 2 == 0) {
    System.out.println("even");  
  } else {
    System.out.println("odd");  
  }
}
```

_Functions_ allow us to name a set of commands:
```
static int addOne(int i) {
  return i + 1;
}
… 
System.out.println(addOne(1));
```

_Classes_ enable us to encapsulate common functionality:
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

_Imports_ let us include encapsulated code:
```
import java.util.ArrayList
… 
class Hi {
    public static void main(String[] args) {
      ArrayList<String> emails = new ArrayList<String>();
      emails.add("a@csumb.edu");
      System.out.println(emails.toString());
```


## Reflect

How do you know if the code your wrote was correct?

Given this simple overview, can you identify the components of your main class?


