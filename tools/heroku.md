# Heroku

Heroku provides an excellent deployment environment.

Heroku provides an example of an _immutable platform_: your changes were deployed as a single entity. You can deploy another change over the top, or roll back the previous deployment, but you cannot change what was deployed. This simplifies reasoning about what's deployed.

## Prerequisites

* Git

## Install

Heroku provides a command line toolkit

```
wget -qO- https://toolbelt.heroku.com/install-ubuntu.sh | sh
```

## Verify

1. Create an app
1. Log into the app on the command line
1. Follow Heroku's documentation to create a hello world Jetty web server
1. Commit the change
1. Deploy the change
1. View logs
1. Observe successful startup
