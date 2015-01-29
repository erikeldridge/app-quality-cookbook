# Launch configuration

## Motivation

Some configuration can be defined when an app is launched. For example, if we want to disable caching in local development, we can communicate an app's environment to the app via command line arguments.


## Goals

* Define launch configuration
* Define an appropriate feature to configure
* Launch an app with configuration


## Steps

1. Define a command line argument we can use to communicate the runtime environment to the previously created web service
2. Identify an appropriate feature to configure
3. Launch the app locally, passing env=development
4. Stage the app, passing env=staging
5. Deploy the app to production, passing env=production


## Verify

* Assert correct behavior via unit and manual testing