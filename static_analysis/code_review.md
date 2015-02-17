# Code review

We can improve the quality of the code base by reviewing changes before they are submitted.

Code reviews also serve to distribute liability, facilitating change.

We'll use Github to explore the idea of code review.

## Prerequisites

* [Github](../tools/github.md)

## Creating a review

1. Identify your reviewers
1. In Github, fork a repository
1. Clone this forked repo
1. Create a branch and implement a change
1. Push the branch to your forked repo
1. Create a pull request and mention your reviewers

## Performing a review

1. If you are a reviewer, click through the “files changed” tab on the pull request to see the diff
1. If you don’t own the repo you’re reviewing code for, just comment on the pull request saying if it looks good or not
1. If you own the repo, and the change looks good, merge the change and close the issue

## Identifying reviewers

We can use a tool like the [review group generator](../exercises/grouper.md) to randomly assign teammates to "review groups". Random assignment ensures teammates maintain experience with the entire code 

Alternatively, we can identify owners of a code base and automatically add them to a review.

If we are not owners, we can review within our team before submitting a review request.

## Review checklist

We can use a checklist to improve consistency in our reviews.

For example:
* Prefer consistency, ie "when in Rome"
* adhere to [style guide](style.md) and [shared principles](../collaboration/principles.md)
* pay special attention to:
	* security-related changes
	* integrations with remote services
	* importing 3rd-party code
* localization
* accessibility
* no dead code
* design review for significant changes
* reviewers participate in design review
* prefer smaller, milestone commits over monolithic changes
* maintain or improve test coverage
* no surprising changes
* add recurring issues to this checklist and static analysis tools
* large changes must run locally

## Related

* [Misko Hevery's code reviewers guide](http://misko.hevery.com/code-reviewers-guide/)
* [Github's documentation on forking a respository and submitting a pull request](https://guides.github.com/activities/forking/)
