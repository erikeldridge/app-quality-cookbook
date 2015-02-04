# Git

## Goal

* Install
* Verify installation

## Motivation

Git is a widely used version control system, supported by many excellent products, like Github and Heroku.

## Prerequisites

* Vagrant

## Steps

### Install

The [virtual machine associated with this book](tools/vagrant.md) will manage installation. Please refer to the [machine's configuration](../Vagrantfile) for more details.

### Verify

1. Create a new directory:
```
$ mkdir foo
```
1. Change into that directory:
```
$ cd foo
```
1. Create a new git repository:
```
$ git init
```
1. Check the status of the repo:
```
$ git status
```
1. Observe the status:
```
On branch master
Initial commit
nothing to commit
```

## Related

