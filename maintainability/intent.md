# Communicating intent

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
