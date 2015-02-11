# Review group generator

Generating rotating groups for code reviews can be tedious

## Steps

### Requirements

Use Java to create a utility for generating a "review group" composed of you and two random reviewers.

This utility should accept the following inputs:
* a comma-separate sequence of all reviewers, including yourself, as a string
* your username as a string
* an integer exercise identifier

The utility should return a review group as a comma-separated sequence of three usernames:

```
$ java Grouper a,b,c,d,e,f e 1
e,c,f
```

Each member of your group should generate the same sequence. For example, user "f" from the example above would see:

```
$ java Grouper a,b,c,d,e,f f 1
e,c,f
```
