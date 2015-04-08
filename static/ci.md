# Continuous integration

[Continuous integration](http://www.thoughtworks.com/continuous-integration) (CI) is the practice of testing and merging code continuously, as opposed to waiting until development completes. We can use CI to run static analysis on a change in review to see if it will "break the build", and on each commit to verify the health of the project. This can help us catch issues early.

We'll use a tool called [Travis](https://travis-ci.org/) for a few reasons: it's relatively modern, so it's enjoyable to use; it integrates seamlessly with Github; it's freely available for open repositories like ours.

Before diving in, I should call out a comparable tool called [Jenkins](https://jenkins-ci.org/). Jenkins, and its predecessor Hudson, are widely used, solid, open-source CI servers, which you'll likely see in documentation and workplaces. We're not using it here because it's relatively difficult to set up and use, but we should be aware of it.

Ok. Here we go. Log into [travis-ci.org](https://travis-ci.org/) with your Github account and allow it to manage your Github repositories. 

Note: travis-ci.org is free, but travis-ci.com is not. We're using the former, but if you ever manage a closed source project you could consider using the latter.

Travis will fetch and display your Github repositories. Find the repository you're using for this exercise and enable CI for it by toggling the switch on the right of the dashboard. Click "Travis CI" in the upper left to navigate home, and then use the "Search all repositories" box to find your repository. Click the repository name to open the repository's dashboard. The url will look like: https://travis-ci.org/&lt;your username&gt;/&lt;your repository name&gt;.

In your local repository, define a _.travis.yml_ config file:

    language: java
    jdk:
        - openjdk7

You can use [Travis' linter](http://lint.travis-ci.org/) to verify your config is correct.

See [Travis' getting started documentation](http://docs.travis-ci.com/user/getting-started/) for more information about this config file.

Commit your config:

    $ git add .travis.yml
    $ git commit -m "Define new travis config file"

Push your change to Github to trigger CI:

    $ git push origin master

Navigate back to the project dashboard you opened above.

You should see your commit listed under the "Current" tab. It will be yellow until the build passes. The entry will turn green if the build passes or red if the build fails.

After the build completes, review the logged output. Look for the Maven commands Travis ran and try to run them locally:

    $ mvn install -DskipTests=true -Dmaven.javadoc.skip=true -B -V
    ...
    $ mvn test -B

Observe that Travis defaults to Maven for Java projects.

Modify your config file to run `mvn verify`:

    language: java
    jdk:
      - openjdk7
    script: mvn verify

Commit this change and push. Observe Travis now runs `mvn verify` after installation instead of `mvn test -B`.

To help catch errors before they break the build, define a pre-push githook (`vim .git/hooks/pre-push`) to run these commands before pushing to github:

    mvn install -DskipTests=true -Dmaven.javadoc.skip=true -B -V
    installed=$?
    
    mvn verify
    verified=$?
    
    if [ ! $installed ] || [ ! $verified ]
    then
        exit 1
    fi

Quick aside: defining a githook to avoid wasting CI time is an example of progressive testing - our tests should become progressively more "expensive" as we get closer to shipping a product.

In development, we want immediate feedback, like IDE hints, lint errors, and quick unit tests to ensure our change works. To avoid breaking the build, but still provide on-demand feedback, we can have CI run full unit and integration test suites before a change merges.

CI can periodically run still more expensive functional tests to ensure our project is healthy on an ongoing basis (because we always want the freedom to apply a critical bug fix on master and deploy).

To simulate production before we ship, we can run live integration tests with real data, define pre-release versions, eg "nightly", alpha", "beta", organize "bug bashes" to manually test the product, etc.

Delivery itself is the final test, which can "cost" us customers if a product doesn't work.

Ok. Let's create a code review and observe CI's validation on the review.

Create and checkout a new branch:

    $ git checkout -b edit_readme

Create and commit a change:

    $ echo "asd" >> README.md
    $ git add README.md
    $ git commit -m "Add text to readme"

Push the branch to Github:

    $ git push origin edit_readme

Navigate to your repo in Github. Click the "Pull requests" link on the right. Click the "New pull request" button to create a pull request. Set master as the base and the branch you just pushed, eg "edit_readme", as the branch to compare.

After creating the pull request, observe Github has added a section it with the Travis details. While the build is in progress, you'll see "Waiting to hear about &lt;git sha&gt;." This status will change to "All is well" when the build succeeds.

Navigate to the Travis dashboard. Observe there's a build for "&lt;your branch&gt; Add text to readme." Click the "Pull requests" tab and observe there's an entry for your pull request.

Once your build passes, merge the change on Github. Observe Travis starts a new build on the master branch to verify "Merge pull request #1 from &lt;your github user name&gt;/&lt;your branch name&gt;".

Congrats! You've just used CI to run static analysis and tests before and after merging.

To learn more about Travis:
* see [Travis' java docs](http://docs.travis-ci.com/user/languages/java/) for more configuration options
* [search Github for .travis.yml files](https://github.com/search?utf8=%E2%9C%93&q=%22language%3A+java%22+language%3Ayml&type=Code&ref=searchresults) to see how other projects use Travis.

Programmatic quality control is much more consistent, but also more brittle, than manual quality control. We can use a balance of both. In general, start with some basic defaults, like the Google style guide, and enforce them programmatically. As you see recurring issues, think about how you could automate the steps you take to prevent them.