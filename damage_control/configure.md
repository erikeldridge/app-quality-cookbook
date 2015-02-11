# Configuration

Sometimes, we can fix an issue quickly by disabling a feature via a configuration change.

A bug that was created by configuration is often resolvable by configuration. If you can correlate a change with the first occurance of the bug, try reverting the change while monitoring the app to see if the bug is resolved.

A feature that is guarded by a configuration check may be resolvable by a configuration change. Beware of stagnant code and side effects.

Maintain configuration code like any other, practice using configuration, test all configuration states, and remove any unused configuration to help keep your configuration system reliable.

## Steps

1. Introduce a bug into a feature
1. Wrap that feature in a dynamic configuration guard
1. Deploy feature
1. Observe bug
1. Edit configuration to disable feature
1. Observe bug (and feature) is no longer visible
