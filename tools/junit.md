# JUnit

We’ll use JUnit for unit testing. 

## Install

1. [Download the jars](https://www.google.com/url?q=https%3A%2F%2Fgithub.com%2Fjunit-team%2Fjunit%2Fwiki%2FDownload-and-Install&sa=D&sntz=1&usg=AFQjCNFRGRj3SWQSR-PIWtjKvpWj6H5F-A)

## Verify

1. Create a file called HiTest.java
1. Define a [passing test](https://www.google.com/url?q=https%3A%2F%2Fgithub.com%2Fjunit-team%2Fjunit%2Fwiki%2FGetting-started&sa=D&sntz=1&usg=AFQjCNGfqvY7aAEqWQ8pzgswAe6Wu9LTyg)
1. Compile the test:
```
javac -cp "./*" HiTest.java
```
1. Run the test:
```
java -cp ".:./*" org.junit.runner.JUnitCore HiTest
```
1. Observe output:
```
JUnit version 4.12-beta-3
Time: 0.008
OK (1 test)
```