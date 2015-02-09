# Linting

We can use "lint" tools to identify syntactic issues in our code

## Diving in

1. Create a file Main.java:

```
class Main {
	public static void main(String[] args) {
	}
}
```

2. Compile

```
javac Main.java -Xlint
```

## Related

* [Java compiler documentation](http://docs.oracle.com/javase/7/docs/technotes/tools/windows/javac.html)
* [Android lint](http://developer.android.com/tools/help/lint.html)
* Android article ["Improving your code with lint"](http://developer.android.com/tools/debugging/improving-w-lint.html)