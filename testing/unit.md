# Unit testing

We've used integration testing to verify `createGroup` works as a whole. We can use unit testing to assert each part of `createGroup` works correctly. We're currently working backward, so this is anticlimactic, but in our future changes we'll start with unit tests and you'll see they make all our other tests much easier to construct.

Assuming the `createGroup` function looks like this:

```java
public ArrayList<String> createGroup(String targetName) throws Exception {
    ArrayList<ArrayList<String>> groups = new ArrayList<>();
    ArrayList<String> tmpGroup = new ArrayList<>();
    for (String name: this.names) {
        tmpGroup.add(name);
        if (tmpGroup.size() == 3) {
            groups.add(tmpGroup);
            tmpGroup = new ArrayList<String>();
        }
    }

    for (ArrayList<String> group : groups) {
        if (group.contains(targetName)) {
            return group;
        }
    }

    throw new Exception("target name not found");
}
```

Conceptually, it's doing a couple things:

* breaking a group up into subgroups of three people
* finding the subgroup containing a given user

Use this conceptual approach to extract two units of functionality:
```java
public ArrayList<ArrayList<String>> createSubgroups() {
    ArrayList<ArrayList<String>> groups = new ArrayList<>();
    ArrayList<String> tmpGroup = new ArrayList<>();
    for (String name: this.names) {
        tmpGroup.add(name);
        if (tmpGroup.size() == 3) {
            groups.add(tmpGroup);
            tmpGroup = new ArrayList<String>();
        }
    }
    return groups;
}

public ArrayList<String> findGroup(ArrayList<ArrayList<String>> groups, String name) {
    for (ArrayList<String> group : groups) {
        if (group.contains(name)) {
            return group;
        }
    }
    return null;
}
```

The createGroup function now looks like this:
    
```java
public ArrayList<String> createGroup(String name) throws Exception {
    ArrayList<ArrayList<String>> groups = createSubgroups()
    ArrayList<String> group = findGroup(groups, name)
    if (group == null) {
        throw new Exception("target name not found");
    } else {
        return group
    }
}
```

Run the functional and integration tests to assert everything still works.

Commit your changes (eg "extract createSubgroups and findGroup methods").

We can now run a couple of our tests at a lower level:

```java
@Test
public void createSubgroups() {
    List<String> names = new ArrayList<String>();
    names.add("a");
    names.add("b");
    names.add("c");
    names.add("d");
    names.add("e");
    names.add("f");
    Generator generator = new Generator(names);
    ArrayList<ArrayList<String>> groups = generator.createSubgroups();
    assertEquals(groups.size(), 2);
    assertEquals(groups.get(0).size(), 3);
}

@Test
public void findGroup() {
    List<String> names = new ArrayList<String>();
    names.add("a");
    names.add("b");
    names.add("c");
    names.add("d");
    names.add("e");
    names.add("f");
    Generator generator = new Generator(names);
    ArrayList<ArrayList<String>> groups = generator.createSubgroups();
    String name = "a";
    ArrayList<String> group = generator.findGroup(groups, name);
    assertEquals(group.contains(name), true);
}
```

Commit your changes:

```nohighlight
$ git add src/test
$ git status
$ git commit -m "define unit tests"
```

Congrats! You now have experience with three common types of testing. Before wrapping up, we’ll touch briefly on a couple practical niceties: fixtures and git hooks.
