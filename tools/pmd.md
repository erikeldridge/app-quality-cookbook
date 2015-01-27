# PMD

We’ll use PMD to help detect problems with our code. 

## Install

1. Download PMD
1. Unzip it

## Verify

1. Add an unused variable to hi.java created in the Java tools 
class Hi {
    public static void main(String[] args) {
        String unused = “unused”;
        ...
run pmd:
./pmd-bin-5.2.2/bin/run.sh pmd -R java-basic,java-unusedcode -d ./
observe PMD’s output:
Avoid unused local variables such as 'unused'.