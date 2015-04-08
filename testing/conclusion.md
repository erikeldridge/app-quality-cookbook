# Conclusion

Our review group generator reduces down to a few steps: shuffling, which we don't need to test because it comes from an external library, creating subgroups, and finding a user in a subgroup. We have unit tests for the last two steps. We have an integration test to assert our units are composed correctly, and a functional test to assert everything prints ok. 

Going forward, we can approach problems in the reverse order: identify the algorithm, write unit tests for each step of the algorithm, write integration tests to assert all the units work well together, and functional test to assert the final product works as intended. When we're working Android, we'll build on our JUnit experience and use UI testing for our functional tests.

We now know our code inside and out and can easily verify it’s still working after making a change. Testing, incremental development and version control are a powerful combination for quality engineering.

We even learned a bit about testability, test fixtures, and test automation.

Please generate your review group for exercise 2, push your branch to your repository, create a pull request and CC your reviewers.