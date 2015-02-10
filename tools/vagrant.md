# Vagrant

This book uses many tools. To establish a consistent environment and simplify installation, we can use [Vagrant](https://www.vagrantup.com) to create and set up a virtual machine.

As a side benefit, Vagrant is widely used for virtual machine automation. One of the principles of quality engineering is to minimize changes between environments. Vagrant helps us do this with respect to the virtual machines that run our services in a data center.

Another side benefit is Vagrant's use of [VirtualBox](https://www.virtualbox.org/), which is also used by [Genymotion](https://www.genymotion.com).

## Install

Follow the [instructions](http://docs.vagrantup.com/v2/installation/index.html) on Vagrant's site to install it

## Create virtual machine

1. Download this book's [Vagrantfile](../Vagrantfile)
1. In the same directory as you saved the Vagrantfile, run the following to create the virtual machine:
```
vagrant up
```
1. Observe vagrant starts logging virtual machine configuration
1. Wait for configuration to complete
1. Observe the virtual machine has created a terminal
1. Log into the terminal using "vagrant" for the username and password
1. Launch Ubuntu's desktop:
```
startx
```
1. Shut down the virtual machine:
```
$ vagrant halt
```




