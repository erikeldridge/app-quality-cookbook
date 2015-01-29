# Dynamic configuration

## Motivation

Some configuration can be served to an app after it is deployed. For example, we may want to deploy a risky change disabled and then gradually enable it.

Note: dynamic configuration involves mutating a system.


## Goals

* Define a dynamic configuration provider
* Define configuration parsing
* Deploy a build with a feature disabled
* Enable the feature dynamically


## Steps

1. Add a new endpoint to the previously created web service
2. Define a configuration provider
3. Ingest and parse the configuration
4. Validate the configuration
5. Define a controller to serve the configuration
6. Modify the previously created Android app to call this endpoint
7. Define application-layer helpers to access the configuration
8. Use the helpers to guard a code path


## Verify

1. Configure a feature to be disabled
2. Observe that your feature is enabled
3. Deploy your app
4. Edit your configuration to enable your feature
5. Observe that your feature is enabled
