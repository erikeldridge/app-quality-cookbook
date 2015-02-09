# Version control

Version control provides us with the ability to create history-aware snapshots of a code base.

These snapshots are named and immutable, which simplifies reasoning.

The snapshots, aka changes, commits, etc., can also hold context, eg the author, a description of the state being commited, the date the change was made, etc, which facilitates investigation.

Best-practices:
* Only commit working code; the master branch should be shippable at all times
* Prefer small commits over monolithic changes
* Do not commit dead code
* Style commit messages according to git guidelines
* Include bug ticket, when available

We can use git and github to explore version control.