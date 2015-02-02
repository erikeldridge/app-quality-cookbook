# Build configuration

## Motivation

Certain types of configuration can be made at build time. For example, when building an app for internal testing, we can set a flag to enable certain features, and then rebuild the app with the flag off to disable and compile away the features before production launch.


## Goals

* Define build configuration
* Define a feature to use build configuration
* Create a configured build

## Prerequisites

* Android experience

## Steps

### Gradle build flavors

Define internal and external [gradle flavors](https://developer.android.com/tools/building/configuring-gradle.html#workBuildVariants)

### Application helpers

Define application-level helpers to access configuration

## Verify

1. Build an internal version of the app
1. Observe that your feature is enabled
1. Build an external version of the app
1. Observe that your feature is disabled