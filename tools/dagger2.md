# Dagger 2

Dagger 2 provides an efficient dependency injection framework by generating all injection code at compile time.

## Prerequisites

* Java
* Maven

## Install

Use Maven to install. Configure pom.xml to include:

```
<repositories>
	<repository>
	  <id>sonatype</id>
	  <name>sonatype-repo</name>
	  <url>https://oss.sonatype.org/content/repositories/snapshots/</url>
	</repository>
</repositories>
<dependencies>
	<dependency>
	  <groupId>com.google.dagger</groupId>
	  <artifactId>dagger</artifactId>
	  <version>2.0-SNAPSHOT</version>
	</dependency>
	<dependency>
	  <groupId>com.google.dagger</groupId>
	  <artifactId>dagger-compiler</artifactId>
	  <version>2.0-SNAPSHOT</version>
	  <optional>true</optional>
	</dependency>
</dependencies>
```

## Verify

mvn compile

## Related

* [Dagger 2 documentation](http://google.github.io/dagger/)
* [Dependency Injection with Dagger 2 (Devoxx 2014)](https://speakerdeck.com/jakewharton/dependency-injection-with-dagger-2-devoxx-2014)
* [DAGGER 2 - A New Type of dependency injection](https://www.youtube.com/watch?v=oK_XtfXPkqw)