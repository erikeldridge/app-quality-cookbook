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

#### Opening a file

```
vim file.txt
```

#### Insert mode

Insert mode is how we add text

Enter insert mode by typing _i_

#### Command mode

Command mode is how we undo, save, exit, etc

Enter command mode by typing _esc_

#### Editing a file

1. Open the file
1. Enter insert mode
1. Make changes

#### Undoing a change

1. Enter command mode
1. Type _:u_ and enter

#### Saving

1. Enter command mode
1. Type _:w_ and hit enter

#### Exit

1. Enter command mode
1. Type _:q_ and hit enter

#### Exit without saving

1. Enter command mode
1. Type _:q!_ and hit enter

## Related

* Vim has many more commands. [Search for vim cheatsheet](https://www.google.com/webhp?#q=vim+cheatsheet)
* [Vim syntax for IntelliJ](https://plugins.jetbrains.com/plugin/164)

