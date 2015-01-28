# PMD

## Motivation

We can use complexity analysis tools to objectively measure flaws in our code.

This objective analysis can make code review more efficient by weeding out problems before they reach review, and catching issues uniformly.

PMD provides a robust complexity analysis that is relatively easy to configure.


## Goals

* Install PMD
* Gain experience using PMD on a simple piece of code


## Install

1. Download [PMD](http://pmd.sourceforge.net/)
1. Unzip it


## Verify

1. Add an unused variable to hi.java created in the [Java installation](tools/java_installation.md) section
```
class Hi {
    public static void main(String[] args) {
        String unused = “unused”;
        ...
```
1. Run pmd:
```
./pmd-bin-5.2.2/bin/run.sh pmd -R java-basic,java-unusedcode -d ./
```
1. Observe PMD’s output:
```
Avoid unused local variables such as 'unused'.
```
