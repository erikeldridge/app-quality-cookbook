# Vagrant

## Motivation

This book covers many tools. We can use Vagrant to create a virtual machine and install all the tools onto it.

As a side benefit, Vagrant is widely used for virtual machine automation. One of the principles of quality engineering is to minimize changes between environments. Vagrant helps us do this with respect to the virtual machines that run our services in a datacenter.

Another side benefit is Vagrant's use of Virtualbox, which is also used by Genymotion.


## Goals

* Install Vagrant
* Create the Ubuntu virtual machine associated with this book


## Steps

1. Follow the instructions on Vagrant's site to install it
1. Download this book's [Vagrantfile](Vagrantfile)
1. Create the virtual machine:
```
vagrant up
```


## Verify

1. Observe the virtual machine has created a terminal
1. Log into the terminal using the username and password "vagrant"
1. Launch Ubuntu's desktop:
```
startx
```
1. Click the "Activities" button in the top navigation
1. Search for an launch the terminal app
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


