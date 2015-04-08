# Lint

The verb [lint](http://en.wikipedia.org/wiki/Lint_%28software%29), as applied to software, refers to scanning code for errors.

Java's compiler provides a relatively simple example:

```nohighlight
$ javac -Xlint:all Main.java
```

For example, define a class that prints "One" if you pass 1 or "Two" if you pass 2:

```java
class Main {
    public static void main(String[] args) {
        switch (Integer.parseInt(args[0])) {
            case 1:
                System.out.println("One");
            case 2:
                System.out.println("Two");
        }
    }
}
```

Compile it:

```nohighlight
$ javac Main.java
```

Run it:

```nohighlight
$ java Main 2
Two
$ java Main 1
One
Two
```

Hmm. That's a bug.

Java's [switch statement](http://docs.oracle.com/javase/tutorial/java/nutsandbolts/switch.html) "falls through" cases unless you add a _break_ statement.

Lint could have helped:

```nohighlight
$ javac -Xlint:all Main.java
Main.java:6: warning: [fallthrough] possible fall-through into case
    case 2:
    ^
1 warning
```

Many languages have linters, eg [Android's lint](http://developer.android.com/tools/help/lint.html), [PyLint](http://www.pylint.org/), [JSLint](http://www.jslint.com/)/[JSHint](http://jshint.com/), etc. I won't dwell on lint here, though, because the style and bug checkers we'll cover are much more powerful than Java's lint.
