# Heroku

## Goal

Heroku provides an excellent deployment environment.

By following the steps in this section, you will gain:
* a Heroku account
* experience with the Heroku command line tools
* experience deploying to Heroku


## Steps

1. Create an account
1. Create an app
1. Log into the app on the command line
1. Follow Heroku's documentation to create a hello world Jetty web server
1. Commit the change
1. Deploy the change


## Verify

1. Query the deployment log
1. Observe your change is listed
1. Query the web service
1. Observe the expected response


## Reflect

Heroku's deployment environment is an example of an _immutable platform_: your changes were deployed as a single entity. You can deploy another change over the top, or roll back the previous deployment, but you cannot change what was deployed. This simplifies reasoning about what's deployed.