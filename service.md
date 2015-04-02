Service development

In this section, we'll
* create a feature switch service using the code you wrote in the [maintainability section](maintainability.md)
* gain experience with build-, launch-, and run-time configuration
* use configuration and version control for damage control
* touch on versioning endpoints for backwards compatability
* use tracing and analytics to monitory app usage
* use dynamic routing to facilitate testing in different environments
* gain experience with fractional, staged, and automated deployment
* expand on our dependency injection (DI) experience using Dagger 2
* learn about redline testing

Create a Java web app

Define a request handler using Jersey

Include a version in your resource path

Observe this version will enable us to preserve backwards compatability if the structure of this endpoint's output changes

Add your feature switch code to this request handler

Modify the app to abstract config as a dependency

Inject the config using Dagger 2

Load config from the resources file

Test using your VM

Deploy the app to Heroku

Observe deploy log

Observe we need to deploy to change config

Load config dynamically (from Github API)

Observe we also get a commit log

Read [Knightmare: A DevOps Cautionary Tale](http://dougseven.com/2014/04/17/knightmare-a-devops-cautionary-tale/)

Use launch and dynamic (fractional deploy) config to switch sources

Observe any client ingesting this config will need to handle the off state

Define staging version of your config service

Run your test script against your VM, then staging, then prod (staged deploy)

Bonus: define canary instance

Split your github config loading into a config service (this is a bit contrived, but it will help demonstrate a couple points)

Define a trace header

Log and relay this trace header

Define a dynamic routing header

Route requests from your front-facing server to your config service using your dynamic routing header

Verify requests using your trace id

Integrate with logly and query for your trace id

Define a script that builds, tests, deploys to staging, tests, deploys to prod, tests (automated deploy)

Rollback your last deploy

Integrate monitoring using New Relic

Observe requests in New Relic

Define redline tests

Define alerts in New Relic

Observe redline visualized

Observe alerts