# Git syntax

The most common commands are:
* _init_ for creating a new repository
* _status_ for checking the state of the repository
* _add_ for "staging" files to commit
* _reset_ for "unstaging" files
* _commit_ for finalizing a commit
* _log_ for seeing all commits 

### Create a new repository

Create a new directory:

```
$ mkdir foo
```

Change into that directory:

```
$ cd foo
```

Create a new git repository:

```
$ git init
```

### Check the status of a repository

```
$ git status

On branch master

Initial commit

nothing to commit
```

### Commit a change to a repository

Create a file:

```
$ echo "hi" > file.txt
```

Observe the file is not yet "tracked" by the git repository:

```
$ git status
...
Untracked files:
  (use "git add <file>..." to include in what will be committed)

	file.txt
```

Add the file to the list of changes "staged" for the next commit:

```
$ git add file.txt
```

Observe the file is now in the list of changes for the next commit:

```
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	new file:   file.txt
...
```

Finalize the commit:

```
git commit
```

Git will launch your default text editor. Write a brief message, eg "Test commit", and exit the text editor.

### View the commit log in a repository

```
$ git log

commit 5b5554c3699f47bf1578759ff5da4d10767e5ff5
Author: Erik Eldridge <erik@example.com>
Date:   Mon Feb 9 09:33:38 2015 -0800

    Test commit 2

commit 5ec5ed660d0a067c8cb61c311f35b9eb15a3a89c
Author: Erik Eldridge <erik@example.com>
Date:   Sun Feb 8 23:24:18 2015 -0800

    Test commit 1
...
```

### Undo a commit

Use reset to undo a commit:

```
$ git reset 5ec5ed660d0a067c8cb61c311f35b9eb15a3a89c
```
