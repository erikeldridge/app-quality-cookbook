# Test using your VM

We can treat our VM as a remote server by configuring _port forwarding_ and then calling our service from our local machine.

Edit your _Vagrantfile_ to uncomment the _forwarded_port_ setting:

```ruby
# Create a forwarded port mapping which allows access to a specific port
# within the machine from a port on the host machine. In the example below,
# accessing "localhost:8080" will access port 80 on the guest machine.
config.vm.network "forwarded_port", guest: 8080, host: 80
```

Reload your VM to apply the configuration change:

```nohighlight
$ vagrant reload
```

Observe port forwarding details logged by the VM as it starts up:

```nohighlight
$ vagrant reload
...
==> default: Forwarding ports...
    default: 8080 => 8080 (adapter 1)
    default: 22 => 2222 (adapter 1)
...
```

After the VM restarts, _ssh_ in and restart your server:

```nohighlight
$ vagrant ssh
$ cd feature-switch-service
$ mvn jetty:run
```

Load your service url in a browser on your local machine:
[http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3](http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3)

Observe your server running in the VM handles the request.

## Learn more

We used an Ubuntu VM for this book to simplify our initial setup and because we needed a desktop for our IDE. Going forward, we can use a tool like [Docker](https://www.docker.com/) to just provide a "container" for a service. We can use [Vagrant](http://docs.vagrantup.com/v2/provisioning/docker.html) or [boot2docker](http://boot2docker.io/) to host the container locally, and a platform like [AWS](https://aws.amazon.com/blogs/aws/cloud-container-management/) or [Kubernetes](http://kubernetes.io/), to host the container remotely.