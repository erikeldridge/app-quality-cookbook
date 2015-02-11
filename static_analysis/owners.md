# Owners

Defining a set of people to automatically add to reviews can facilitate reviewer selection, debugging, and code standardization.

Owners are responsible for the code they own and should provide timely feedback for contributors.

## Steps

### Tooling

A simple example:
1. add a file called "OWNERS" to the directory containing files to control
1. when making changes, add everyone listed in the OWNERS file to the review
1. only merge once an owner has reviewed and approved the change

## Related

* Automation for determing owners and approvers
* [Gerrit's access control](https://gerrit-documentation.storage.googleapis.com/Documentation/2.10/access-control.html)
* [Github's permission levels](https://help.github.com/articles/permission-levels-for-an-organization-repository/)