# PMD

We’ll use PMD to help detect problems with our code. 


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