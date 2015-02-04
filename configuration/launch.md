# Launch configuration

## Goals

* Define launch configuration
* Define an appropriate feature to configure
* Launch an app with configuration

## Motivation

Some configuration can be defined when an app is launched. For example, if we want to disable caching in local development, we can communicate an app's environment to the app via command line arguments.

## Steps

### Define flags

Define a command line argument we can use to communicate the runtime environment to the previously created web service

### Define application helpers

Identify an appropriate feature to configure

### Set flags

Launch the app locally, passing env=development

Stage the app, passing env=staging

Deploy the app to production, passing env=production


## Verify

* Assert correct behavior via unit and manual testing