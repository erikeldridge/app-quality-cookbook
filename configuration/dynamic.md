# Dynamic configuration

Some configuration can be served to an app after it is deployed. For example, we may want to deploy a risky change disabled and then gradually enable it.

## Prerequisites

* Android

## Steps

1. Define configuration service
1. Define controller to serve configuration to external clients
1. Modify client to call configuration service
1. Validate configuration
1. Define application-layer helpers to access the configuration
1. Use the helpers to guard a code path
1. Configure a feature to be disabled
2. Observe feature is disabled
4. Edit configuration to enable feature
5. Observe feature is enabled
