# Git hooks

We can use a [git hook](http://git-scm.com/book/en/v2/Customizing-Git-Git-Hooks) to ensure we always run at least a sanity check test before committing our code. A git hook is a script that executes in response to a git events like commit, push, pull, etc. We’ll take a step in the direction of test automation by defining a pre-commit hook to run our tests before each commit.

Building on our Unix experience, copy git’s sample pre-commit file:

    $ cp .git/hooks/pre-commit.sample .git/hooks/pre-commit

Open the file for editing:

    $ vim .git/hooks/pre-commit

Replace the contents with an invocation of our test script:

    #!/bin/bash
    
    if ! mvn test; then
      echo "Failing tests! Aborting commit!"
      exit 1
    fi

The first line tells the terminal to run this file using Bash. Then we have some conditional logic that fails if our test script failed with a non-zero exit code.

Define a breaking change, eg:

    ...
    @Test
    public void groupSize() {
        Generator generator = new Generator(this.names);
        ArrayList<ArrayList<String>> groups = 
          generator.createSubgroups();
        assertEquals(groups.size(), 1);
    ...

Try to commit this change:

    $ git add test.sh
    $ git commit -m “add breaking change”
    [INFO] Scanning for projects...
    ...
    Failed tests:   groupSize(com.mycompany.app.GeneratorTest): expected:<2> but was:<1>
    
    Tests run: 4, Failures: 1, Errors: 0, Skipped: 0
    ...
    Failing tests! Aborting commit!

Observe that the change is still staged and has not been committed:

    $ git status
    $ git log
