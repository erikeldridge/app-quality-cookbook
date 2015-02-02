# Debugging

## Motivation

If possible, being able to step through code execution can be a quick way to identify the cause of a bug.

## Goals

* Debug Java on the command line
* Debug Java from IntelliJ
* Debug Android from Android Studio

## Prerequisites

* Experience with Java

## Steps

### Overview

Debuggers generally have a few features in common:
* _Breakpoints_ stop code execution on a particular line
* _Stepping into_ a function
* _Stepping over_ a function
* _Stepping out_ of a function
* _Continuing_ jumps to the next breakpoint
* _Listing_ the lines around the breakpoint

JDB is Java's command line debugger.

### Create file to debug

For example:
```
class Runner {
	public static void main(String[] args) {
		System.out.print(args);
	}
}
```

### Compile with debug flag

```
$ javac -g Runner.java
```

### Launch debugger

```
$ jdb Runner
```

### Breakpoints

Stop execution at a given line:

```
> stop at Runner:3
```

### Start execution

```
> run
```

### Observe execution stops at the breakpoint

```
Breakpoint hit: "thread=main", Bug.main(), line=3 bci=0
3             System.out.println(args);
main[1]
```

### List

List the lines around the breakpoint:

```
main[1] list
1     class Bug {
2         public static void main(String[] args) {
3 =>          System.out.println(args);
4         }
5     }
```

### Dump a variable

```
main[1] dump args
 args = {
"foo"
}
```

## Related

* [IntelliJ's debugging documentation](https://www.jetbrains.com/idea/help/debugging.html)

