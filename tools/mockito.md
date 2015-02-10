# Mockito

Mockito is a Java testing tool that allows us to assert and override how objects in our code are used.

## Prerequisites

* Java
* JUnit
* Maven

## Install

Use Maven to install and run. Edit pom.xml:

```
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.2</version>
      <configuration>
        <source>1.7</source><!-- support Mockito's static import -->
        <target>1.7</target>
      </configuration>
    </plugin>
  </plugins>
</build>
...
</dependencies>
  <dependency>
    <groupId>org.mockito</groupId>
    <artifactId>mockito-all</artifactId>
    <version>1.9.5</version>
    <scope>test</scope>
  </dependency>
</dependencies>
```

## Verify

Edit AppTest.java, created in the validation step of the Maven tool description:

```
import static org.mockito.Mockito.*;
...
public void testVerification() {
	List mockedList = mock(List.class);
	mockedList.add("one");
	verify(mockedList).add("two");	
}
```

Observe failure:

```
$ mvn test
...
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running com.mycompany.app.AppTest
Tests run: 1, Failures: 1, Errors: 0, Skipped: 0, Time elapsed: 0.222 sec <<< FAILURE!

Results :

Failed tests:   testVerification(com.mycompany.app.AppTest): 

Tests run: 1, Failures: 1, Errors: 0, Skipped: 0

[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 1.242s
[INFO] Finished at: Tue Feb 10 07:55:29 UTC 2015
[INFO] Final Memory: 6M/15M
[INFO] ------------------------------------------------------------------------
...
```

## Related

* [Mockito's site](http://mockito.org/)
* [Maven repo for Mockito](http://mvnrepository.com/artifact/org.mockito/mockito-all/1.9.5)