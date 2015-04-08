# Code review

We can improve the quality of the code base by reviewing changes before they are submitted.

Code reviews also serve to distribute liability, facilitating change.

We'll use Github to explore the idea of code review.

## Create a review

Identify your reviewers using the review group generator for section 1:

    $ java Generator erik@example.com 1
    [joy@example.com, javier@example.com, erik@example.com]

In Github, fork each of your group's repositories.

Clone each forked repo.

In each, create a branch and implement a fix for the issue you reported earlier.

Push the branch to your forked repo.

Create a pull request and mention your reviewers.

For example, [search Square's pull requests for usages of "cc"](https://github.com/search?utf8=%E2%9C%93&q=cc+language%3Ajava+type%3Apr+user%3Asquare&type=Issues&ref=searchresults) to see how people submitting pull requests ask for feedback from specific people.

## Perform a review

If you are a reviewer, click through the “files changed” tab on the pull request to see the diff ([example](https://github.com/square/okhttp/pull/1488/files)). 

If you don’t own the repo you’re reviewing code for, just comment on the pull request saying if it looks good or not.

If you own the repo, and the change looks good, merge the change and close the issue.

## Identify reviewers

Code review involves multiple people, which introduces organizational complexity. We can use the code review group generator to simplify this process. Random assignment ensures teammates maintain experience with the entire code base.

Alternatively, we can pre-identify owners of a code base and automatically add them to a review. Github uses "contributors" for this.

Some projects may define a two-step review process: first a member of my team reviews and then an owner approves.

## Review checklist

We can use a checklist to improve consistency in our reviews.

For example:
* Prefer consistency
* Adhere to [style guide](style.md) and [shared principles](../collaboration/principles.md)
* Pay special attention to:
  * security-related changes
  * integrations with remote services
  * importing 3rd-party code
* Localization
* Accessibility
* No dead code
* Design review for significant changes
* Reviewers participate in design review
* Prefer smaller, milestone commits over monolithic changes
* Maintain or improve test coverage
* No surprising changes
* Add recurring issues to this checklist and static analysis tools
* Large changes must run locally

## Learn more

* [Misko Hevery's code reviewers guide](http://misko.hevery.com/code-reviewers-guide/)
* [Github's documentation on forking a respository and submitting a pull request](https://guides.github.com/activities/forking/)