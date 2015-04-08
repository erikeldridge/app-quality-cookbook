# IntelliJ

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

## Review group generator exercise

Code review is an important aspect of quality engineering. We'll learn more about code review when we explore version control, but we can use the task of selecting reviewers as an opportunity to dive further into Java and use our IDE.

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

How did your experience writing Java in Vim compare with the IDE? Did you have less errors in one versus the other?

## Learn more

* [JetBrain's documentation on setting a breakpoint](https://www.jetbrains.com/idea/help/creating-breakpoints.html)
* [JetBrain's documentation on starting the debugger session](https://www.jetbrains.com/idea/help/starting-the-debugger-session.html)
* [JetBrain's documentation on the debug tool window](https://www.jetbrains.com/idea/help/debug-tool-window.html)
* [JVM args for remote debugging](http://stackoverflow.com/a/4150943)
* Comparable debuggers: gdb for C/C++, Chrome's developer tools for JavaScript, byebug for Ruby, etc.
