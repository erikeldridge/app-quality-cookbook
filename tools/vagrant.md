# Vagrant

## Motivation

This book uses many tools. To establish a consistent environment and simplify installation, we can use a virtual machine. First you install [Vagrant](https://www.vagrantup.com), and then use the configuration linked below to generate the virtual machine.

As a side benefit, Vagrant is widely used for virtual machine automation. One of the principles of quality engineering is to minimize changes between environments. Vagrant helps us do this with respect to the virtual machines that run our services in a data center.

Another side benefit is Vagrant's use of [VirtualBox](https://www.virtualbox.org/), which is also used by [Genymotion](https://www.genymotion.com).


## Goals

* Install Vagrant
* Create the Ubuntu virtual machine associated with this book


## Steps

1. Follow the [instructions](http://docs.vagrantup.com/v2/getting-started/index.html) on Vagrant's site to install it
1. Download this book's [Vagrantfile](../Vagrantfile)
1. In the same directory as you saved the Vagrantfile, run the following to create the virtual machine:
```
vagrant up
```
1. Observe the virtual machine has created a terminal
1. Log into the terminal using "vagrant" for the username and password
1. Launch Ubuntu's desktop:
```
startx
```
1. Click the "Activities" button in the top navigation
1. Search for and launch the Terminal app
1. List the contents of your home directory:
```
$ ls ~
```
1. Verify you have checkstyle, junit, mockito jars
1. Verify you have the installed apps:
```
$ javac
$ git
```
1. Search for Firefox and launch to verify you have a browser
1. Search for IntelliJ and launch to verify IntelliJ installation
1. Shut down the virtual machine:
```
$ vagrant halt
```




