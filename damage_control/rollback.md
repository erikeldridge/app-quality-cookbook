# Rolling back

If a current version is buggy, we can re-release a previous, working version to fix an issue. 

Rolling back is a relatively heavyweight option, however, so we should verify the fix and rule out alternatives before proceeding.

## Steps

1. Identify appropriate failure mode, ie not config-based
1. Implement and deploy bug
1. Observe issue
1. Verify bug did not exist in previous release
1. Rollback release
1. Verify fix
1. Reflect on costs
