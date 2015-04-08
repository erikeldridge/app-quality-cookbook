# Fixtures

Sometimes we need to use a set of data in several of our tests. For example, the code above instantiates a list of names repeatedly. JUnit defines something called a [test fixture](https://github.com/junit-team/junit/wiki/Test-fixtures), which can help.

Define a class member for the fixture you’d like to provide to your tests:

```java
public class AppTest {
    private List<String> names;
    …
```

To populate the list of names before every test, we can “annotate” a function with @Before:

```java
import org.junit.Before;
...
    @Before
    public void setUp() {
        this.names = new ArrayList<String>();
        names.add("a");
        names.add("b");
        names.add("c");
        names.add("d");
        names.add("e");
        names.add("f");
    }
```

Now we can simplify our tests:

```java
@Test
public void groupSize() {
    Generator generator = new Generator(this.names);
    ...
}
```

Commit this code to your branch as above
