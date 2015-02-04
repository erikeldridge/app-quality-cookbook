# Vim

## Goals

* Install
* Dive in

## Motivation

Having a tool to quickly edit a file from the Unix command line is handy.

For example, we frequently log into remote Unix machines, where IntelliJ is not installed. However, vim generally is. 

## Prerequisites

* Unix

## Steps

### Install

The [virtual machine associated with this book](tools/vagrant.md) will manage installation. Please refer to the [machine's configuration](../Vagrantfile) for more details.

### Diving in

Open a terminal

Create a file with some text in it:

```
echo "hi" > file.txt
```

Open the file:

```
vim file.txt
```

Enter "insert mode" by typing _i_ and hitting enter

Delete the "hi" text

Enter "command mode" by typing _esc_

Exit the file without saving by typing _:q!_ and _enter_

Re-open the file

Enter insert mode

Add some text

Save the file by typing _:w_ and _enter_

Exit by typing _:q_ and _enter_

Search online for "vim cheatsheet"

Observe there are many more commands
