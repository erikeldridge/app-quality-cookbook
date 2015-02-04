# Rolling back

## Goals

* Identify appropriate failure mode, ie not config-based
* Implement and deploy bug
* Observe issue
* Rollback release
* Verify fix
* Reflect costs

## Motivation

If a current version is buggy, we can re-release a previous, working version to fix an issue. Rolling back is a relatively heavyweight option, however, so we should verify the fix and rule out alternatives before proceeding.
