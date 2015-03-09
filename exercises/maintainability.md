# Maintainability

Think about one fundamental question when writing or reviewing code: How am I going to test this? – [Misko Hevery](http://misko.hevery.com/attachments/Guide-Writing%20Testable%20Code.pdf)

Code that is easier to read is easier to understand. Code that is easier to understand is easier to maintain. Code that is easier to maintain has less bugs. Code that has few bugs is less frightening to deploy. – [@javi](https://twitter.com/javi)

The phrase "maintainability" refers to the ease with which we can maintain our code. Testability and readability are two important aspects of maintainability.

This exercise covers four common aspects of maintainable code:
* avoiding side effects
* communicating intent
* adhering to known patterns
* doing one thing well

We'll build a configuration generator to explore these ideas. We'll start by implementing code with low maintainability and then refactor to make it more maintainable.

## Spaghetti

Some code is unpleasant to work with because its dependencies and influences are poorly defined. Such code is difficult to modify without side effects because it is difficult to understand. A common term for such code is spaghetti.

Generate a new maven quickstart project.

Define a main method that accepts the following inputs:
* an operating system name
* an operating system version
* a random user id

The method should read configuration from a file. I use [YAML](http://yaml.org/) for the examples in this exercise and call the file features.yml.

Example configuration:

    feature_a
      os: android
    feature_b:
      percentage: 10
    feature_c:
      os: android
      percentage: 50


Java refers to a file like this as a [resource](http://docs.oracle.com/javase/tutorial/deployment/webstart/retrievingResources.html), and Maven defines a [standard "resources" directory](http://maven.apache.org/guides/getting-started/index.html#How_do_I_add_resources_to_my_JAR), eg /home/vagrant/my-app/src/main/resources/features.yml

The "os" field defines the operating system name for which a feature is enabled. The "percentage" field defines the percentage of people for which a feature is enabled.

For example, if the os is "android" and the percentage is "50", 50% of android users should see this feature:

    $ java Main 1357246 android
    feature_a:true
    feature_b:false
    feature_c:true

Implement your function with minimal code.

For example:

    public class Main {
        public static void main( String[] args ) throws IOException {
            Integer i = Integer.valueOf(args[0]);
            String o = args[1];
            Double v = Double.valueOf(args[2]);
            String g = Main.class.getClassLoader().getResource("rules.yml").getFile();
            YamlReader r = new YamlReader(new FileReader(g));
            Map<String, Map<String, String>> fs = r.read(Map.class);
            Map<String, Boolean> s = new HashMap<>();
            for (String n : fs.keySet()) {
                Map<String, String> f = fs.get(n);
                s.put(n, (!f.containsKey("os") || f.get("os").equals(o))
                        && (!f.containsKey("percentage") || i % 100 < Integer.valueOf(f.get("percentage"))));
            }
            for (Map.Entry<String, Boolean> f : s.entrySet()) {
                System.out.println(f.getKey() + ":" + f.getValue());
            }
        }
    }

For the code above, I'm using a library called [YamlBeans](https://github.com/EsotericSoftware/yamlbeans) to parse yaml. I import it into my project via Maven:
    
    ...
    <dependency>
        <groupId>com.esotericsoftware.yamlbeans</groupId>
        <artifactId>yamlbeans</artifactId>
        <version>1.09</version>
    </dependency>
    ...
    
Commit your code once you get it working.

Now we'd like to get experience working with this code. To make this as realistic as possible, either take a break to clear your head, eg read through [SourceMaking's description of spaghetti code](http://sourcemaking.com/antipatterns/spaghetti-code) and pretend 6 months have passed, or pair with another person on your team and work with each other's code.

Update the code to enable features based on os version. For example, given a config like this:
    
    ...
    feature_c:
      os: android
      version: 2.3
      percentage: 50

And you pass a lower version to the program:

    $ java Main 1357246 android 2

You should see:

    ...
    feature_c:false

Commit your change.

Generate a review group for exercise 3.

Push your branch to review once complete, CCing your review group. This will be a point of comparison for later.

### Reflect

Was it easy to modify your code to add support for os version?

As a reviewer, can you understand what the code is doing?

## Avoiding side effects

We literally can’t write a JUnit test for this code because the output of this function is printed to standard out.

To fix this, we can extract functionality from the main method to another method that returns a value we can test.

[Functional programming](http://en.wikipedia.org/wiki/Functional_programming) lends itself to testing by preferring functions which take input, return output, and avoid side effects. Java isn't a functional language, strictly speaking, but we can still take a "functional" approach and limit side effects. In our case, writing to standard out is a side effect.

Isolate this side effect by pulling the code that determines if a feature is enabled out of the main function.

For example:

    public class Main {
        public Map<String, Boolean> s(Integer i, String o, Double v)
                throws FileNotFoundException, YamlException {
            String g = Main.class.getClassLoader()
              .getResource("rules.yml").getFile();
            YamlReader r = new YamlReader(new FileReader(g));
            Map<String, Map<String, String>> fs = r.read(Map.class);
            Map<String, Boolean> s = new HashMap<>();
            for (String n : fs.keySet()) {
                Map<String, String> f = fs.get(n);
                s.put(n, (!f.containsKey("os") || f.get("os").equals(o))
                        && (!f.containsKey("version") || v.compareTo(Double.valueOf(f.get("version"))) >= 0)
                        && (!f.containsKey("percentage") || i % 100 < Integer.valueOf(f.get("percentage"))));
            }
            return s;
        }
        public static void main( String[] args ) throws IOException {
            Integer i = Integer.valueOf(args[0]);
            String o = args[1];
            Double v = Double.valueOf(args[2]);
            Main m = new Main();
            for (Map.Entry<String, Boolean> f : m.s(i, o, v).entrySet()) {
                System.out.println(f.getKey() + ":" + f.getValue());
            }
        }
    }

We can now write a test for the function:

    @Test
    public void test() throws FileNotFoundException, YamlException {
        Main m = new Main();
        Map<String, Boolean> a = m.s(1357246, "android", 2.3);
        Map<String, Boolean> e = new HashMap<>();
        e.put("feature_c", false);
        e.put("feature_b", false);
        e.put("feature_a", true);

        assertEquals(e, a);
    }

[Maven's standard layout](http://maven.apache.org/guides/introduction/introduction-to-the-standard-directory-layout.html) defines a src/test/resources directory we can use:

    $ cd my-app
    $ cp my-app/src/main/resources/features.yml my-app/src/test/resources/features.yml
    $ ls my-app/src/test/resources/
    features.yml
    $ mvn test
    ...

    -------------------------------------------------------
     T E S T S
    -------------------------------------------------------
    Running com.mycompany.app.MainTest
    Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.049 sec

    Results :

    Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

    [INFO] ------------------------------------------------------------------------
    [INFO] BUILD SUCCESS
    ...

## Dependency injection

The practice of [dependency injection](http://en.wikipedia.org/wiki/Dependency_injection) refers to identifying things, like data sources, that our code depends on and providing them explicitly to our code. This becomes especially valuable as our dependencies become more costly to query, eg fetching from a remote data store.

The act of identifying dependencies helps break code up into logical sections. The "seams" between these sections are often appropriate places to test.

In our case, we can treat our feature set as a dependency and pass it into our constructor:

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

We can split dependencies from the code, and use those dependencies, without much fuss. Sometimes this is sufficient, however, several frameworks exist, eg [Guice](https://github.com/google/guice), [Dagger 2](http://google.github.io/dagger/), etc, to help organize the dependencies in our project.

Having identified dependencies, we’re now free to [mock](http://en.wikipedia.org/wiki/Mock_object) them in our tests. We’ll use [Mockito](http://mockito.org/) as our mocking framework.

Use Maven to install. Edit your project’s pom.xml:

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

Add the following tests:

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

Note these tests are not dependent on loading a file in our test environment. We can have our mocks return any content that makes sense in a test, and verify the mock object was called correctly.

Commit your changes once your tests pass.

## Communicating intent

In general, writing the least code to satisfy requirements often results in the most simple product.

However, programming languages exist largely to be readable by the people who have to maintain code. Otherwise, we'd just write in binary.

So, let's modify our variable and function names to be more [self-documenting](http://en.wikipedia.org/wiki/Self-documenting).

Use IntelliJ's refactoring support to easily and consistently rename objects. Click a variable to rename, navigate to Refactor > Rename ... in the top bar, type a new name, and observe the variable is renamed everywhere.

For example:

    public class FeatureSelector {
        Map<String, Map<String, String>> features;
        public FeatureSelector(Map<String, Map<String, String>> features) {
            this.features = features;
        }
        public Map<String, Boolean> select(Integer id, String os, Double version)
                throws FileNotFoundException, YamlException {
            Map<String, Boolean> selected = new HashMap<>();
            for (String name : features.keySet()) {
                Map<String, String> feature = features.get(name);
                selected.put(name, 
                    (!feature.containsKey("os") || feature.get("os").equals(os))
                    && (!feature.containsKey("version") || version.compareTo(Double.valueOf(feature.get("version"))) >= 0)
                    && (!feature.containsKey("percentage") || id % 100 < Integer.valueOf(feature.get("percentage"))));
            }
            return selected;
        }
        public static void main( String[] args ) throws IOException {
            Integer id = Integer.valueOf(args[0]);
            String os = args[1];
            Double version = Double.valueOf(args[2]);
            String path = FeatureSelector.class.getClassLoader().getResource("features.yml").getFile();
            YamlReader features = new YamlReader(new FileReader(path));
            FeatureSelector selector = new FeatureSelector(features.read(Map.class));
            for (Map.Entry<String, Boolean> feature : selector.select(id, os, version).entrySet()) {
                System.out.println(feature.getKey() + ":" + feature.getValue());
            }
        }
    }


Run your tests to verify the refactor didn't break anything. Feel the confidence that comes from having tests :)

Refactor your tests to make them more readable too:

    public class FeatureSelectorTest {

        @Test
        public void selectShouldEnable() throws FileNotFoundException, YamlException {
            Map<String, Map<String, String>> features = mock(Map.class);
            Map<String, String> feature = new HashMap<>();
            feature.put("os", "android");
            when(features.get("feature_a")).thenReturn(feature);
            Set<String> set = new HashSet<>();
            set.add("feature_a");
            when(features.keySet()).thenReturn(set);
            FeatureSelector selector = new FeatureSelector(features);
            Map<String, Boolean> actual = selector.select(1357246, "android", 2.3);
            verify(features).get("feature_a");
            Map<String, Boolean> expected = new HashMap<>();
            expected.put("feature_a", true);
            assertEquals(expected, actual);
        }
    ...

It's possible to go overboard in an attempt to make code completely self-documenting. As a general rule, write the minimum to be readable.

## Design patterns

A design pattern "is a general repeatable solution to a commonly occurring problem in software design." – [SourceMaking](http://sourcemaking.com/design_patterns)

We can take advantage of shared understanding by organizing our code according to existing patterns.

Inappropriate application of patterns can reduce maintainability, however, so we'll use if a pattern only if it helps simplify code and communication.

The conditional logic in our selection method is difficult to read and modify. Martin Fowler would describe this as a [code smell](http://martinfowler.com/bliki/CodeSmell.html), "a surface indication that usually corresponds to a deeper problem in the system". Many code smells are [associated](http://www.industriallogic.com/wp-content/uploads/2005/09/smellstorefactorings.pdf) with patterns which can be used to alleviate the smell.

Let's use the compose method pattern to reduce our conditional complexity.

Take a step back from the code and list the steps required to fulfill our requirements:

    ...
    isEnabled(input, feature) {
        return osMatches(input.os, feature.os) 
            and osVersionMatches(input.osVersion, feature.osVersion) 
            and percentageMatches(input.percentage, feature.percentage)
    selectFeatures(features, input)
        for feature in features
            selectedFeatures[feature] = isEnabled(input, feature)
        return selectedFeatures
    main(args)
        input = requireInput(args)
        features = createFeatureMap()
        selected = selectFeatures(features, input)
        output = formatOutput(selected)
        print(output)

Iteratively refactor by:
1. [extracting a method](http://sourcemaking.com/refactoring/extract-method)
2. testing to make sure everything is still working, and adding tests for new code
3. committing your change
4. repeating steps 1-3 until all methods are extracted

The `requireInput` method seems like a good place to start:

    ...
    public static void main( String[] args ) throws IOException {
        Input input = requireInput(args);
        ...
        for (Map.Entry<String, Boolean> feature : selector.select(input.id, input.os, input.version).entrySet()) {
            System.out.println(feature.getKey() + ":" + feature.getValue());
        }
    }

    public static Inputs requireInput(String[] args) {
        Integer id = Integer.valueOf(args[0]);
        String os = args[1];
        Double version = Double.valueOf(args[2]);
        return new Inputs(id, os, version);
    }
    ...
    class Input {
        public final Integer id;
        public final String os;
        public final Double version;

        public Input(Integer id, String os, Double version) {
            this.id = id;
            this.os = os;
            this.version = version;
        }

        @Override
        public boolean equals(Object o) {
            if (this == o) return true;
            if (o == null || getClass() != o.getClass()) return false;

            Input input = (Input) o;

            if (!id.equals(input.id)) return false;
            if (!os.equals(input.os)) return false;
            if (!version.equals(input.version)) return false;

            return true;
        }

        @Override
        public int hashCode() {
            int result = id.hashCode();
            result = 31 * result + os.hashCode();
            result = 31 * result + version.hashCode();
            return result;
        }
    }
    ...
    @Test
    public void requireInputsShouldDefineInputs() {
        Input actual = FeatureSelector.requireInput(new String[]{"1", "b", "2.3"});
        Input expected = new Input(1, "b", 2.3);
        assertEquals(expected, actual);
    }
    ...

The method must be [static](http://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html) because we need to call it before we instantiate the `FeatureSelector` class.

The `hashCode` and `equals` functions enable us compare instances of the `Input` class, eg `assertEquals(expected, actual)`. Use IntelliJ's generator to generate them: Code > Generate ... > equals() and hashCode()

For the next function, `createFeatureMap`, extract the code using IntelliJ's refactoring tools: Refactor > Extract > Method ...

    ...
    public static void main( String[] args ) throws IOException {
        Input input = requireInput(args);
        Map<String, Map<String, String>> features = createFeatureMap()
        ...
    }
    ...
    public Map<String, Map<String, String>> createFeatureMap() throws IOException {
        String path = FeatureSelector.class.getClassLoader().getResource("features.yml").getFile();
        YamlReader features = new YamlReader(new FileReader(path));
        return features.read(Map.class);
    }
    ...
    @Test
    public void createFeatureMapShouldCreateMap() throws FileNotFoundException, YamlException {
        Map<String, Map<String, String>> actual = FeatureSelector.createFeatureMap();
        Map<String, HashMap<String, String>> expected = new HashMap<>();
        HashMap<String, String> feature = new HashMap<>();
        feature.put("os", "android");
        feature.put("version", "2.3");
        feature.put("percentage", "50");
        expected.put("feature_c", feature);
        assertEquals(expected, actual);
    }

The `createFeatureMap` function is a bit tricker to test because it reads from a file. We could either use Maven's test resources to load the file or mock `getResource` and `FileReader` file loading. Because mocking these objects would be relatively complicated, most of the code is external library calls, and we only need one test, using test resources makes sense.

The test above assumes src/test/resources/features.yml has the following config:

    feature_c:
      os: android
      version: 2.3
      percentage: 50

After extracting all your methods and composing functions your code may look something like this:

    public class FeatureSelector {
        Map<String, Map<String, String>> features;
    
        public FeatureSelector(Map<String, Map<String, String>> features) {
            this.features = features;
        }
    
        public static void main( String[] args ) throws IOException {
            Input input = requireInput(args);
            Map<String, Map<String, String>> features = createFeatureMap();
            FeatureSelector selector = new FeatureSelector(features);
            Map<String, Boolean> selected = selector.select(input);
            System.out.println(formatOutput(selected));
        }
    
        public static Input requireInput(String[] args) {
            Integer id = Integer.valueOf(args[0]);
            String os = args[1];
            Double version = Double.valueOf(args[2]);
            return new Input(id, os, version);
        }
    
        public static Map<String, Map<String, String>> createFeatureMap() throws FileNotFoundException, YamlException {
            String path = FeatureSelector.class.getClassLoader().getResource("features.yml").getFile();
            YamlReader features = new YamlReader(new FileReader(path));
            return features.read(Map.class);
        }
    
        public Map<String, Boolean> select(Input input)
                throws FileNotFoundException, YamlException {
            Map<String, Boolean> selected = new HashMap<>();
            for (String name : features.keySet()) {
                Map<String, String> feature = features.get(name);
                selected.put(name, isEnabled(input, feature));
            }
            return selected;
        }
    
        public boolean isEnabled(Input input, Map<String, String> feature) {
            return isOsEnabled(feature.get("os"), input.os)
                    && isVersionEnabled(feature.get("version"), input.version)
                    && isPercentageEnabled(feature.get("percentage"), input.id);
        }
    
        public boolean isOsEnabled(String featureOs, String inputOs) {
            return featureOs == null || featureOs.equals(inputOs);
        }
    
        public boolean isVersionEnabled(String featureVersion, Double inputVersion) {
            return featureVersion == null || Double.valueOf(featureVersion).compareTo(inputVersion) <= 0;
        }
    
        public boolean isPercentageEnabled(String featurePercentage, Integer inputId) {
            return featurePercentage == null || inputId % 100 < Integer.valueOf(featurePercentage);
        }
    
        public static String formatOutput(Map<String, Boolean> selected) throws FileNotFoundException, YamlException {
            StringBuilder builder = new StringBuilder();
            for (Map.Entry<String, Boolean> feature : selected.entrySet()) {
                builder
                    .append(feature.getKey())
                    .append(":")
                    .append(feature.getValue())
                    .append("\n");
            }
            return builder.toString();
        }
    }
    ...
    public class FeatureSelectorTest {
    
        @Test
        public void requireInputsShouldDefineInputs() {
            Input actual = FeatureSelector.requireInput(new String[]{"1", "b", "2.3"});
            Input expected = new Input(1, "b", 2.3);
            assertEquals(expected, actual);
        }
    
        @Test
        public void createFeatureMapShouldCreateMap() throws FileNotFoundException, YamlException {
            Map<String, Map<String, String>> actual = FeatureSelector.createFeatureMap();
            Map<String, Map<String, String>> expected = new HashMap<>();
            HashMap<String, String> feature = new HashMap<>();
            feature.put("os", "android");
            feature.put("version", "2.3");
            feature.put("percentage", "50");
            expected.put("feature_c", feature);
            assertEquals(expected, actual);
        }
    
        @Test
        public void formatOutputShouldFormatOutput() throws FileNotFoundException, YamlException {
            HashMap<String, Boolean> selected = new HashMap<>();
            selected.put("feature_c", true);
            String expected = "feature_c:true\n";
            String actual = FeatureSelector.formatOutput(selected);
            assertEquals(expected, actual);
        }
    
        @Test
        public void isEnabledShouldSetEnabled() {
            Map<String, Map<String, String>> features = new HashMap<>();
            HashMap<String, String> feature = new HashMap<>();
            feature.put("os", "android");
            features.put("feature_c", feature);
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isEnabled(new Input(1357246, "android", 2.3), feature);
            assertEquals(true, actual);
        }
    
        @Test
        public void isEnabledShouldSetDisabled() {
            Map<String, Map<String, String>> features = new HashMap<>();
            HashMap<String, String> feature = new HashMap<>();
            feature.put("os", "android");
            features.put("feature_c", feature);
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isEnabled(new Input(1357246, "ios", 2.3), feature);
            assertEquals(false, actual);
        }
    
        @Test
        public void isOsEnabledShouldBeTrueIfDefinedAndEqual() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isOsEnabled("a", "a");
            assertEquals(true, actual);
        }
    
        @Test
        public void isOsEnabledShouldBeTrueIfUndefined() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isOsEnabled(null, "a");
            assertEquals(true, actual);
        }
    
        @Test
        public void isOsEnabledShouldBeFalseIfDefinedAndUnequal() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isOsEnabled("a", "b");
            assertEquals(false, actual);
        }
    
        @Test
        public void isVersionEnabledShouldBeTrueIfDefinedAndEqual() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isVersionEnabled("2.3", 2.3);
            assertEquals(true, actual);
        }
    
        @Test
        public void isVersionEnabledShouldBeTrueIfDefinedAndLessThan() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isVersionEnabled("2", 2.3);
            assertEquals(true, actual);
        }
    
        @Test
        public void isVersionEnabledShouldBeTrueIfUndefined() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isVersionEnabled(null, 2.3);
            assertEquals(true, actual);
        }
    
        @Test
        public void isVersionEnabledShouldBeFalseIfDefinedAndGreaterThan() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isVersionEnabled("2.3", 2.0);
            assertEquals(false, actual);
        }
    
        @Test
        public void isPercentageEnabledShouldBeTrueIfDefinedAndLessThan() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isPercentageEnabled("10", 1);
            assertEquals(true, actual);
        }
    
        @Test
        public void isPercentageEnabledShouldBeFalseIfDefinedAndGreaterThan() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isPercentageEnabled("10", 11);
            assertEquals(false, actual);
        }
    
        @Test
        public void isPercentageEnabledShouldBeTrueIfUndefined() {
            Map<String, Map<String, String>> features = new HashMap<>();
            FeatureSelector selector = new FeatureSelector(features);
            boolean actual = selector.isPercentageEnabled(null, 11);
            assertEquals(true, actual);
        }
    
        @Test
        public void selectShouldEnable() throws FileNotFoundException, YamlException {
            Map<String, Map<String, String>> features = mock(Map.class);
            Map<String, String> feature = new HashMap<>();
            feature.put("os", "android");
            when(features.get("feature_a")).thenReturn(feature);
            Set<String> set = new HashSet<>();
            set.add("feature_a");
            when(features.keySet()).thenReturn(set);
            FeatureSelector selector = new FeatureSelector(features);
            Map<String, Boolean> actual = selector.select(new Input(1357246, "android", 2.3));
            verify(features).get("feature_a");
            Map<String, Boolean> expected = new HashMap<>();
            expected.put("feature_a", true);
            assertEquals(expected, actual);
        }
    
        @Test
        public void selectShouldDisable() throws FileNotFoundException, YamlException {
            Map<String, Map<String, String>> features = mock(Map.class);
            Map<String, String> feature = new HashMap<>();
            feature.put("os", "android");
            when(features.get("feature_a")).thenReturn(feature);
            Set<String> set = new HashSet<>();
            set.add("feature_a");
            when(features.keySet()).thenReturn(set);
            FeatureSelector selector = new FeatureSelector(features);
            Map<String, Boolean> actual = selector.select(new Input(1357246, "ios", 2.3));
            verify(features).get("feature_a");
            Map<String, Boolean> expected = new HashMap<>();
            expected.put("feature_a", false);
            assertEquals(expected, actual);
        }
    }

## Doing one thing well

[Unix philosophy](http://www.faqs.org/docs/artu/ch01s06.html) recommends we "do one thing and do it well".

Code that performs a single action is relatively easy to reason about and maintain.

To paraphrase the principles of [separation of concerns](http://en.wikipedia.org/wiki/Separation_of_concerns) and [single-responsibility](http://en.wikipedia.org/wiki/Single_responsibility_principle), each piece of our code should perform a distinct role.

While refactoring our code to use the compose method pattern, we broke it up into several functions, each performing a single action.

If you're having trouble naming a function, the function is probably trying to do several things.

Some single-purpose functions can be so basic we won't need to test them. For example:

    int addOne(int i) {
        return i + 1;
    }

However, other functions can be single-purpose, but still relatively complex:

    public boolean isPercentageEnabled(String featurePercentage, Integer inputId) {
        return featurePercentage == null || inputId % 100 < Integer.valueOf(featurePercentage);
    }

Review your code with single-purpose functions and classes in mind. Is there anything still trying to do too much?

## Conclusions

We've written spaghetti code and then cleaned it up by avoiding side effects, identifying and abstracting dependencies, communicating intent, designing with patterns in mind, and refactoring our methods to be single-purpose.

The next time you interact with spaghetti code, either directly or in review, you can use this experience to provide effective feedback.

Commit your changes and create another review. This should be the second review for this exercise (so we can easily compare before and after).
