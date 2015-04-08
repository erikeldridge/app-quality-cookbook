# Spaghetti

Some code is unpleasant to work with because its dependencies and influences are poorly defined. Such code is difficult to modify without side effects because it is difficult to understand. A common term for such code is spaghetti.

To explore this concept, we're going to define a function that generates [feature switch](http://martinfowler.com/bliki/FeatureToggle.html) configuration, a set of boolean values we can use to enable/disable features.

Generate a new maven quickstart project.

Define a main method that accepts the following inputs:
* an operating system name
* an operating system version
* a random user id

The method should read configuration from a file. I use [YAML](http://yaml.org/) for the examples in this exercise and call the file features.yml.

Example configuration:

```yaml
feature_a:
  os: android
feature_b:
  percentage: 10
feature_c:
  os: android
  percentage: 50
```

Java refers to a file like this as a [resource](http://docs.oracle.com/javase/tutorial/deployment/webstart/retrievingResources.html), and Maven defines a [standard "resources" directory](http://maven.apache.org/guides/getting-started/index.html#How_do_I_add_resources_to_my_JAR), eg /home/vagrant/my-app/src/main/resources/features.yml

The "os" field defines the operating system name for which a feature is enabled. The "percentage" field defines the percentage of people for which a feature is enabled.

For example, if the os is "android" and the percentage is "50", 50% of android users should see this feature:

```nohighlight
$ java Main 1357246 android
feature_a:true
feature_b:false
feature_c:true
```

Implement your function with minimal code.

For example:

```java
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
```

For the code above, I'm using a library called [YamlBeans](https://github.com/EsotericSoftware/yamlbeans) to parse yaml. I import it into my project via Maven:
    
```xml
...
<dependency>
    <groupId>com.esotericsoftware.yamlbeans</groupId>
    <artifactId>yamlbeans</artifactId>
    <version>1.09</version>
</dependency>
...
```
    
Commit your code once you get it working.

Now we'd like to get experience working with this code. To make this as realistic as possible, either take a break to clear your head, eg read through [SourceMaking's description of spaghetti code](http://sourcemaking.com/antipatterns/spaghetti-code) and pretend 6 months have passed, or pair with another person on your team and work with each other's code.

Update the code to enable features based on os version. For example, given a config like this:
    
```yaml
...
feature_c:
  os: android
  version: 2.3
  percentage: 50
```

And you pass a lower version to the program:

```nohighlight
$ java Main 1357246 android 2
```

You should see:

```nohighlight
...
feature_c:false
```

Commit your change.

Generate a review group for exercise 3.

Push your branch to review once complete, CCing your review group. This will be a point of comparison for later.

Was it easy to modify your code to add support for os version?

As a reviewer, can you understand what the code is doing?
