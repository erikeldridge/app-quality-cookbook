# Functional testing

We’ll build on our Unix experience for this and use the Bash scripting tool to collect the output from our program and assert its correctness.

## A quick dive into Bash

You can run Bash commands directly in your terminal. You can also save the commands in a file and then run the file.

### Variables

```bash
$ foo=bar
$ echo $foo
bar
```

### Operators

```bash
$ i=$((1 + 1)) # addition
$ echo $i
2
```

### Loops

```bash
$ for ((i=0; i<5; i++)); do echo $i; done
0
1
2
3
4
```

### Conditions

```bash
$ i=1; if [[ $i = 1 ]]; then echo true; else echo false; fi
```

### Functions

```bash
$ function foo () { echo $1; };
$ foo bar
bar
```

## Define test

The code below assumes your review group generator runs like this:

```nohighlight
$ java Main e 2
[b, f, e]
```

Our generator should always return the same value given the same inputs. We can assert the code is working correctly by verifying this.

Create a file for running our test:

```nohighlight
$ vim test.sh
```

In this file, add the command to run your generator and compare the output with known valid value:

```bash
function assert () {
  if [[ "$1" = "$2" ]]; then
    echo success
    exit 0
  else
    echo error: $1 does not equal $2
    exit 1
  fi
}

actual=`java Main e 2`
expected="[b, f]"

assert "$actual" "$expected"
```

Note that the value we’re asserting is incorrect. We’ll run the failing test first to make sure the test actually works:

```nohighlight
$ bash test.sh 
"[b, f, e] does not equal [b, f@]"
```

Fix the value and re-run:

```nohighlight
$ bash test.sh 
success
```

With this test we can easily verify the output hasn’t changed. And we know from looking at the output that it’s correct. We can use integration and unit testing to assert correctness of the code that generates this output.

Now would be a good time to cut a branch and commit your changes:

```nohighlight
$ git branch exercise_2
$ git checkout exercise_2
$ git status
$ git add test.sh
$ git commit -m "define a script for functional testing"
```
