# Design patterns

A design pattern "is a general repeatable solution to a commonly occurring problem in software design." – [SourceMaking](http://sourcemaking.com/design_patterns)

We can take advantage of shared understanding by organizing our code according to existing patterns.

Inappropriate application of patterns can reduce maintainability, however, so we'll use if a pattern only if it helps simplify code and communication.

The conditional logic in our selection method is difficult to read and modify. Martin Fowler would describe this as a [code smell](http://martinfowler.com/bliki/CodeSmell.html), "a surface indication that usually corresponds to a deeper problem in the system". Many code smells are [associated](http://www.industriallogic.com/wp-content/uploads/2005/09/smellstorefactorings.pdf) with patterns which can be used to alleviate the smell.

Let's use the compose method pattern to reduce our conditional complexity.

Take a step back from the code and list the steps required to fulfill our requirements:

```java
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
```

Iteratively refactor by:
1. [extracting a method](http://sourcemaking.com/refactoring/extract-method)
2. testing to make sure everything is still working, and adding tests for new code
3. committing your change
4. repeating steps 1-3 until all methods are extracted

The `requireInput` method seems like a good place to start:

```java
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
```

The method must be [static](http://docs.oracle.com/javase/tutorial/java/javaOO/classvars.html) because we need to call it before we instantiate the `FeatureSelector` class.

The `hashCode` and `equals` functions enable us compare instances of the `Input` class, eg `assertEquals(expected, actual)`. Use IntelliJ's generator to generate them: Code > Generate ... > equals() and hashCode()

For the next function, `createFeatureMap`, extract the code using IntelliJ's refactoring tools: Refactor > Extract > Method ...

```java
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
```

The `createFeatureMap` function is a bit tricker to test because it reads from a file. We could either use Maven's test resources to load the file or mock `getResource` and `FileReader` file loading. Because mocking these objects would be relatively complicated, most of the code is external library calls, and we only need one test, using test resources makes sense.

The test above assumes src/test/resources/features.yml has the following config:

```yaml
    feature_c:
      os: android
      version: 2.3
      percentage: 50
```

After extracting all your methods and composing functions your code may look something like this:

```java
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
```
