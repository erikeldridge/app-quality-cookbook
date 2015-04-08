# Unix

Unix, and the Unix-like Linux, are widely used, high-quality operating systems.

Some examples:
* Mac's are commonly used for development, and OS X is a Unix operating system
* Linux is the most common operating system for servers
* Android is based on Linux

We'll use a popular variety of Linux called Ubuntu for this class. We’ll use Vagrant to create a virtual machine in which we can run Ubuntu.

As a side benefit, we’ll gain experience with Vagrant, which is widely used for virtual machine automation. One of the principles of quality engineering is to minimize changes between environments. Vagrant helps us do this with respect to the virtual machines that run locally and in our data centers.

Vagrant's use of VirtualBox, which is also used by a fast Android emulator called Genymotion, is another side benefit.

Follow the instructions on Vagrant's site to install Vagrant.

Download this book's Vagrantfile.

Note: some browsers save the file as html. Open the Vagrantfile and ensure it’s contents look the same as what you see in the browser.

Open a terminal and navigate to the directory where you saved the Vagrantfile.

Run the following to create the virtual machine:

```nohighlight
$ vagrant up
```

You should see lots of text printed to the terminal as vagrant configures the virtual machine. Wait for configuration to complete.

The virtual machine should launch a terminal. Log in using “vagrant” as the username and password. From this terminal, start the desktop:

```nohighlight
$ startx
```

We’ll use this desktop later when we work with IntelliJ, but let’s dive into the Unix file system first.

Secure shell (SSH) is a common way of safely connecting with a remote service. We can use it to log into our virtual machine:

```nohighlight
$ vagrant ssh
```

## Navigation

Print the current working directory

```nohighlight
$ pwd
```

List contents of directory

```nohighlight
$ ls
```

Change directory

```nohighlight
$ cd foo
```

Go to your home directory

```nohighlight
$ cd ~
```

## File management

Make a directory

```nohighlight
$ mkdir foo
```

Move a file

```nohighlight
$ mv foo.txt bar.txt
```

Remove a file

```nohighlight
$ rm foo.txt
```

Copy a file

```nohighlight
$ cp foo.txt bar.txt
```

## Learn more

Search for unix cheat sheet

Read [The Art of Unix Programming](http://catb.org/~esr/writings/taoup/html/), esp The Basics of Unix Philosophy
