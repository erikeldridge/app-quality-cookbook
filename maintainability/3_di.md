# Dependency injection

The practice of [dependency injection](http://en.wikipedia.org/wiki/Dependency_injection) refers to identifying things, like data sources, that our code depends on and providing them explicitly to our code. This becomes especially valuable as our dependencies become more costly to query, eg fetching from a remote data store.

The act of identifying dependencies helps break code up into logical sections. The "seams" between these sections are often appropriate places to test.

In our case, we can treat our feature set as a dependency and pass it into our constructor:

```java
public class Main {
    Map<String, Map<String, String>> fs;
    public Main(Map<String, Map<String, String>> fs) {
        this.fs = fs;
    }
    ...
    public static void main( String[] args ) throws IOException {
        ...
        String g = Main.class.getClassLoader().getResource("features.yml").getFile();
        YamlReader r = new YamlReader(new FileReader(g));
        Main m = new Main(r.read(Map.class));
        ...
    }
}
```

We can split dependencies from the code, and use those dependencies, without much fuss. Sometimes this is sufficient, however, several frameworks exist, eg [Guice](https://github.com/google/guice), [Dagger 2](http://google.github.io/dagger/), etc, to help organize the dependencies in our project.

Having identified dependencies, we’re now free to [mock](http://en.wikipedia.org/wiki/Mock_object) them in our tests. We’ll use [Mockito](http://mockito.org/) as our mocking framework.

Use Maven to install. Edit your project’s pom.xml:

```xml
<build>
  <plugins>
    <plugin>
      <groupId>org.apache.maven.plugins</groupId>
      <artifactId>maven-compiler-plugin</artifactId>
      <version>3.2</version>
      <configuration>
        <!-- Mockito's static import requires Java 1.7 -->
        <source>1.7</source>
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

Edit your tests to use mock objects:

```java
import static org.mockito.Mockito.*;

...

@Test
public void s() throws FileNotFoundException, YamlException {
    Map<String, Map<String, String>> fs = mock(Map.class);
    Map<String, String> f = new HashMap<>();
    f.put("os", "android");
    when(fs.get("feature_a")).thenReturn(f);
    Set<String> s = new HashSet<>();
    s.add("feature_a");
    when(fs.keySet()).thenReturn(s);
    Main m = new Main(fs);
    Map<String, Boolean> a = m.s(1357246, "android", 2.3);
    verify(fs).get("feature_a");
    Map<String, Boolean> e = new HashMap<>();
    e.put("feature_a", true);
    assertEquals(e, a);
}

@Test
public void f() throws FileNotFoundException, YamlException {
    Map<String, Map<String, String>> fs = mock(Map.class);
    Map<String, String> f = new HashMap<>();
    f.put("os", "android");
    when(fs.get("feature_a")).thenReturn(f);
    Set<String> s = new HashSet<>();
    s.add("feature_a");
    when(fs.keySet()).thenReturn(s);
    Main m = new Main(fs);
    Map<String, Boolean> a = m.s(1357246, "ios", 2.3);
    verify(fs).get("feature_a");
    Map<String, Boolean> e = new HashMap<>();
    e.put("feature_a", false);
    assertEquals(e, a);
}
```

Note these tests are not dependent on loading a file in our test environment. We can have our mocks return any content that makes sense in a test, and verify the mock object was called correctly.

Commit your changes once your tests pass.