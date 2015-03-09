# Introduction

This section focuses on introducing some basic quality engineering concepts and tools we can use to explore them.

We'll start with a simple drawing exercise touching on the fundamentals of development and quality control.

Next, we'll dive into Unix, which provides the basis for many of the operating systems we interact with. We'll use it to establish a consistent development environment for all the examples used in this book.

We'll use Vagrant to define a Linux virtual machine. Vagrant is widely used for automating virtual machine management.

We'll explore Vim, a common text editor, and use it to dive into Java, which we'll use for a variety of applications throughout this course.

We'll use the Java compiler as a simple static analysis tool to start analyzing our code.

We'll wrap up by introducing an integrated development environment (IDE) called IntelliJ we can use to improve the quality of our code.

## A drawing exercise

Quality engineering concepts can be obscured by technical details. We can avoid this by starting with simple tools. Grab a pen and paper.

### Solo development

We often develop alone when we're getting started with an idea.

1. Think of a shape
1. Draw this shape on paper
1. Verify the shape drawn was the one intended

This shape is our product.

Was it produced correctly?

Because you are the "customer" as well as the "developer" the feedback loop is very small. The cost of shipping this product was a piece of paper and some time to draw the shape. Your audience is just yourself.

### Collaboration

Pair up with another person.

1. Think of another shape
1. Ask the person to draw it
1. Verify the shape was the one intended

Was it produced correctly?

What steps did you go through to verify it was produced correctly?

If you were to do this exercise again, would you do anything differently to ensure the shape was drawn correctly?

For example, would written requirements have helped?

## Unix

Unix, and the Unix-like Linux, are widely used, high-quality operating systems.

Some examples:
* Mac's are commonly used for development, and OS X is a Unix operating system
* Linux is the most common operating system for servers
* Android is based on Linux

We'll use a popular variety of Linux called Ubuntu for this class. We’ll use Vagrant to create a virtual machine in which we can run Ubuntu.

As a side benefit, we’ll gain experience with Vagrant, which is widely used for virtual machine automation. One of the principles of quality engineering is to minimize changes between environments. Vagrant helps us do this with respect to the virtual machines that run locally and in our data centers.

Vagrant's use of VirtualBox, which is also used by a fast Android emulator called Genymotion, is another side benefit.

Follow the instructions on Vagrant's site to install Vagrant.

Download this book's Vagrantfile.

Note: some browsers save the file as html. Open the Vagrantfile and ensure it’s contents look the same as what you see in the browser.

Open a terminal and navigate to the directory where you saved the Vagrantfile.

Run the following to create the virtual machine:

    $ vagrant up

You should see lots of text printed to the terminal as vagrant configures the virtual machine. Wait for configuration to complete.

The virtual machine should launch a terminal. Log in using “vagrant” as the username and password. From this terminal, start the desktop:

    $ startx

We’ll use this desktop later when we work with IntelliJ, but let’s dive into the Unix file system first.

Secure shell (SSH) is a common way of safely connecting with a remote service. We can use it to log into our virtual machine:

    $ vagrant ssh

## Navigation

Print the current working directory

    $ pwd

List contents of directory

    $ ls

Change directory

    $ cd foo

Go to your home directory

    $ cd ~

## File management

Make a directory

    $ mkdir foo

Move a file

    $ mv foo.txt bar.txt

Remove a file

    $ rm foo.txt

Copy a file

    $ cp foo.txt bar.txt

### Learn more

Search for unix cheat sheet

Read [The Art of Unix Programming](http://catb.org/~esr/writings/taoup/html/), esp The Basics of Unix Philosophy

## Vim

Vim is a broadly available text editor. Having a tool to quickly edit a file from the Unix command line is handy.

### Insert mode

Insert mode is how we add text

Enter insert mode by typing _i_

### Command mode

Command mode is how we undo, save, exit, etc

Enter command mode by typing _esc_

### Editing a file

1. Open the file
1. Enter insert mode
1. Make changes

### Undoing a change

1. Enter command mode
1. Type _:u_ and enter

### Saving

1. Enter command mode
1. Type _:w_ and hit enter

### Exit

1. Enter command mode
1. Type _:q_ and hit enter

### Exit without saving

1. Enter command mode
1. Type _:q!_ and hit enter

### Learn more

Emacs and Nano are comparable text editors.

Ubuntu uses Nano as its default editor. [Set vim as your default text editor](http://vim.wikia.com/wiki/Set_Vim_as_your_default_editor_for_Unix) for consistency.

Vim has a ton of commands. Search for vim cheat sheet.

## Java

Java provides a mature, widely used programming language and runtime.

In terms of quality engineering, many tools are written in Java and/or designed to support it.

### Install

This book’s Vagrantfile should have installed Java for you, but in case that didn’t work [download and install the Java Development Kit (JDK) from Sun](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)

Verify installation by checking the version of the Java compiler:

    $ javac -version
    javac 1.7.0_75

### Build and run

Using Vim, create a file called Main.java:

    $ vim Main.java

All Java applications have a [main method](http://docs.oracle.com/javase/tutorial/getStarted/application/index.html) Java calls to kick off the program. Copy/paste the code below into your Main.java:

    class Main {
      public static void main(String[] args) {
        System.out.println("Hi");
      }
    }

Compile using the java compiler:

    $ javac Main.java

View the resulting Main.class:

    $ ls
    Main.java Main.class

Run it:

    $ java Main
    Hi

Repeat these steps to play with the Java syntax below.

### Variables

Variables let us assign a value to a name:

    int number = 0;

### Operators

Operators enable us to perform actions on variables:

    number = 1; // assignment
    System.out.println(number);
    number = i + 1; // addition
    System.out.println(number);

### Loops

Loops enable us to iterate over a collection of values:

    String[] values = {"a", "b", "c"};
    for (int i = 0; i < values.length; i++) {
      System.out.println(values[i]);
    }

### Conditions

Conditions allow us to require something to be true:

    for (int i = 0; i < 10; i++) {
      if (i % 2 == 0) {
        System.out.println("even");  
      } else {
        System.out.println("odd");  
      }
    }

### Functions 

Function allow us to name a set of commands:

    static int addOne(int i) {
      return i + 1;
    }
    … 
    System.out.println(addOne(1));

### Classes

Classes enable us to encapsulate common functionality:

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

### Imports

Imports let us include encapsulated code:

    import java.util.ArrayList
    … 
    class Main {
        public static void main(String[] args) {
          ArrayList<String> emails = new ArrayList<String>();
          emails.add("a@csumb.edu");
          System.out.println(emails.toString());

### Exercise

Write a program that accepts two numeric arguments and uses the Number class to determine if the sum of the numbers is even:

    $ java Main 1 3
    true
    $ java Main 10 5
    false

## IntelliJ

We've now written Java using a text editor.

Having experience with a text editor is good in case we need to quickly edit a file on the command line, but using an IDE will increase our ability to write quality code in numerous ways, namely code completion and syntax highlighting.

IntelliJ is best-of-breed and widely used, so we'll benefit from a large community. It provides code completion and syntax highlighting in a variety of languages, and it can be configured to run external tools.

The Vagrantfile associated with this book should have installed IntelliJ in your virtual machine. In case it hasn't, 
[download and install IntelliJ](https://www.jetbrains.com/idea/download/).

Verify installation, and familiarize yourself with Jetbrain's documentation, by following [Jetbrain’s instructions for a hello world app](https://www.jetbrains.com/idea/help/creating-and-running-your-first-java-application.html).

## Debuggers

Debuggers enable us to examine running code.

Debuggers generally allow us to:
* Set a "breakpoint" to pause execution 
* Step through execution line by line
* Navigate among execution scopes
* Show the contents of a variable in the current scope

Debuggers exist for many languages. IntelliJ provides an excellent debugger we can use to explore these concepts in Java.

Use the review group generator below as an opportunity to explore:
1. Setting a breakpoint
1. Launching the debugger
1. Stepping over a line
1. Stepping into a function
1. Stepping out of a function
1. Continuing to the next breakpoint
1. Showing the contents of a variable
1. Executing arbitrary code

### Learn more

* [JetBrain's documentation on setting a breakpoint](https://www.jetbrains.com/idea/help/creating-breakpoints.html)
* [JetBrain's documentation on starting the debugger session](https://www.jetbrains.com/idea/help/starting-the-debugger-session.html)
* [JetBrain's documentation on the debug tool window](https://www.jetbrains.com/idea/help/debug-tool-window.html)
* [JVM args for remote debugging](http://stackoverflow.com/a/4150943)
* Comparable debuggers: gdb for C/C++, Chrome's developer tools for JavaScript, byebug for Ruby, etc.

## Review group generator exercise

Code review is an important aspect of quality engineering. We'll learn more about code review when we explore version control, but we can use the task of selecting reviewers as an opportunity to dive further into Java and use our IDE.

### Requirements

Create a utility for generating a "review group" composed of you and two reviewers.

This utility should accept the following inputs:
* your username as a string
* an integer exercise identifier

The utility should return a review group as a sequence of three usernames:

    $ java Generator e 1
    e,c,f

For a given exercise identifier, each member of your group should generate the same sequence. For example, user "f" from the example above would see:

    $ java Generator f 1
    e,c,f

Changing the exercise identifier should change the composition of the group generated:

    $ java Generator f 2
    a,f,b

### Reflect

How did your experience writing Java in Vim compare with the IDE? Did you have less errors in one versus the other?

## Conclusion

We used a simplified drawing exercise to start exploring quality engineering themes such as the cost of delivery vs the cost of process, iterative development, collaboration, and static analysis.

We created a Linux virtual machine using Vagrant. We used this virtual machine to establish a consistent development environment. Going forward, you could use a similar set of tooling to emulate a remote service for local development.

We learned how to navigate the Linux file system and manipulate files from the command line, an invaluable skill set for remote debugging.

We wrapped up by starting to use and IDE for development and debugging. IDEs provide powerful tools like code completion and syntax highlighting we can use to produce high quality code.




