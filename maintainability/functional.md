# Avoiding side effects

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