# Maven

Maven simplifies and standardizes build configuration. It is commonly used, and other tools like Gradle can pull from Maven repositories.

## Steps

### Install

```
$ sudo apt-get install maven
```

### Verify

```
$ mvn -version
Apache Maven 3.0.5
Maven home: /usr/share/maven
Java version: 1.7.0_75, vendor: Oracle Corporation
Java home: /usr/lib/jvm/java-7-openjdk-i386/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.13.0-45-generic", arch: "i386", family: "unix"
```

Create a quickstart project:

```
$ mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=my-app -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
...
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 29.494s
[INFO] Finished at: Tue Feb 10 05:28:29 UTC 2015
[INFO] Final Memory: 10M/25M
[INFO] ------------------------------------------------------------------------
...
$ ls -R my-app/
my-app/:
pom.xml  src

my-app/src:
main  test

my-app/src/main:
java

my-app/src/main/java:
com

my-app/src/main/java/com:
mycompany

my-app/src/main/java/com/mycompany:
app

my-app/src/main/java/com/mycompany/app:
App.java

my-app/src/test:
java

my-app/src/test/java:
com

my-app/src/test/java/com:
mycompany

my-app/src/test/java/com/mycompany:
app

my-app/src/test/java/com/mycompany/app:
AppTest.java
```
