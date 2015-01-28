# Version control

Version control provides us with the ability to take a history-aware snapshot of a code base.

These snapshots are named and immutable, which simplifies reasoning. For example, we can deploy version 123 and know exactly what is included.

The snapshots, aka changes and commits, can also hold context, eg the author, a description of the state being commited, the date the change was made, etc, which facilitates investigation.

Best-practice: only commit working code; the main branch should always be deployable.