# Deploy your service

Create a free [Heroku](https://www.heroku.com/) account.

Install the [Heroku toolbelt](https://toolbelt.heroku.com). (If you're using this book's [Vagrantfile](http://erikeldridge.gitbooks.io/app-quality-cookbook/content/Vagrantfile), it should install the toolbelt in your VM.)

Verify the toolbelt is installed by running:

```nohighlight
$ heroku
```

Follow [Jersey's instructions to deploy your service to Heroku](https://jersey.java.net/documentation/latest/user-guide.html#deploy-it-on-heroku).

Call your service as before, but using the generated host name:

```nohighlight
$ curl -v "https://young-depths-7217.herokuapp.com/feature_switch_config?id=123&os=android&version=2.3"
...
{"feature_c":true,"feature_b":false,"feature_a":true}
```

Congratulations! You now have experience deploying a service to [production](http://en.wikipedia.org/wiki/Development_environment_%28software_development_process%29).

## Observe deploy log

Heroku is great in part because it provides an [immutable service environment](http://martinfowler.com/bliki/ImmutableServer.html). We cannot change our code after it's deployed.

Heroku [increments a version number](https://devcenter.heroku.com/articles/releases) each time we deploy a release.

You can see these versions in the release log:

```nohighlight
$ heroku releases
=== young-depths-7217 Releases
v6  Deploy 0f3e4dd                           erik@example.com  2015/04/06 01:19:27 (~ 20m ago)
v5  Attach HEROKU_POSTGRESQL_GREEN resource  erik@example.com  2015/04/06 01:19:27 (~ 20m ago)
v4  Set DATABASE_URL config vars             erik@example.com  2015/04/06 01:19:26 (~ 20m ago)
v3  Set JAVA_OPTS config vars                erik@example.com  2015/04/06 01:19:26 (~ 20m ago)
v2  Enable Logplex                           erik@example.com  2015/04/05 23:02:41 (~ 2h ago)
v1  Initial release                          erik@example.com  2015/04/05 23:02:41 (~ 2h ago)
```

Heroku is also great because of it's close integration with git. We release specific versions of our project, as identified by their git hash. You can see this association by looking at the git log and heroku release log:

```nohighlight
$ git log -n 1 --oneline
0f3e4dd Integrate feature selector
$ heroku releases -n 1
=== young-depths-7217 Releases
v6  Deploy 0f3e4dd  erik@example.com  2015/04/06 01:19:27 (~ 27m ago)
```
