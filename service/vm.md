# Test using your VM

We can treat our VM as a remote server by configuring _port forwarding_ and then calling our service from our local maching.

Edit your _Vagrantfile_ to uncomment the _forwarded_port_ setting:

    # Create a forwarded port mapping which allows access to a specific port
    # within the machine from a port on the host machine. In the example below,
    # accessing "localhost:8080" will access port 80 on the guest machine.
    config.vm.network "forwarded_port", guest: 8080, host: 80

Reload your VM to apply the configuration change:

    $ vagrant reload

Observe port forwarding details logged by the VM as it starts up:

    $ vagrant reload
    ...
    ==> default: Forwarding ports...
        default: 8080 => 8080 (adapter 1)
        default: 22 => 2222 (adapter 1)
    ...

After the VM restarts, _ssh_ in and restart your server:

    $ vagrant ssh
    $ cd feature-switch-service
    $ mvn jetty:run

Load [http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3](http://localhost:8080/feature_switch_config?id=123&os=android&version=2.3) in a browser on your local machine. Observe your server running in the VM handles the request.

We used an Ubuntu VM for this book to simplify our initial setup and because we needed a desktop. Going forward, we can use a tool like [Docker](https://www.docker.com/) to just provide a "container" for a service. We can use [Vagrant](http://docs.vagrantup.com/v2/provisioning/docker.html) to host the container locally, and a platform like [AWS](https://aws.amazon.com/blogs/aws/cloud-container-management/) or [Kubernetes](http://kubernetes.io/), to host the container remotely.