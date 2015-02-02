# Linting

## Motivation

We can use linting to identify syntactic issues in our code.

## Goals

* Lint Java
* Lint Android

## Steps

### Linting Java

1. Create a file Runner.java:

```
class Runner {
	public static void main(String[] args) {
	}
}
```

2. Compile

```
javac Runner.java -Xlint
```

### Linting Android


## Related

* [Java compiler documentation](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html)
* [Android lint](http://developer.android.com/tools/help/lint.html)
* Android article ["Improving your code with lint"](http://developer.android.com/tools/debugging/improving-w-lint.html)
* [Wikipedia's article on software linting](http://en.wikipedia.org/wiki/Lint_%28software%29)