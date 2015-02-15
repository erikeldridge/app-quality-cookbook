# Git syntax

Here are some common commands:
* `git init` for creating a new repository
* `git status` for seeing which files have changed
* `git add` for "staging" files to commit
* `git commit` for committing changes to a repository
* `git log` for seeing all commits
* `git show` for showing the changes in a commit
* `git diff` for seeing what's changed
* `git reset` for "unstaging" changes

## Create a new repository

1. Create a new directory:

        $ mkdir foo

1. Change into that directory:

        $ cd foo

1. Create a new git repository:

        $ git init


## See which files have changed

        $ git status
        
        On branch master
        
        Initial commit
        
        nothing to commit

## Commit a change to a repository

1. Create a file:

        $ echo "hi" > file.txt

1. Observe the file is not yet "tracked" by the git repository:

        $ git status
        ...
        Untracked files:
          (use "git add <file>..." to include in what will be committed)
        
        	file.txt

1. Add the file to the list of changes "staged" for the next commit:

        $ git add file.txt

1. Observe the file is now in the list of changes for the next commit:

        $ git status
        On branch master
        Changes to be committed:
          (use "git reset HEAD <file>..." to unstage)
        
        	new file:   file.txt
        ...

1. Finalize the commit:

        $ git commit
Git will launch your default text editor. Write a brief message, eg "Add file", and exit the text editor.

## See all commits in a repository

        $ git log
        commit 230d2a431184c93da0d7a5f7fbeaa00e168e005e
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 11:27:45 2015 -0800
        
            Add file
        ...

## Show the changes in a commit

        $ git show 230d2a431184c93da0d7a5f7fbeaa00e168e005e
        commit 230d2a431184c93da0d7a5f7fbeaa00e168e005e
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 11:27:45 2015 -0800
        
            Add file
        
        diff --git a/file.txt b/file.txt
        new file mode 100644
        index 0000000..d72af31
        --- /dev/null
        +++ b/file.txt
        @@ -0,0 +1 @@
        +hi

## See what's changed

1. Create another change, eg:

        $ echo "world" >> file.txt

1. Run `git diff` to see what has changed:

        $ git diff
        diff --git a/file.txt b/file.txt
        index d72af31..87e05f0 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         hi
        +world

## Unstage changes

1. Run `git add` to stage the change made above
1. Run `git status` to see what's staged for the next commit
1. Run `git reset` to unstage:

        $ git reset
        Unstaged changes after reset:
        M file.txt

## Undoing a commit

1. Add the change made above and commit
1. Run `git log` to see all commits:

        $ git log
        commit 7c7981e34176e105848403dad215126b321c6c21
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 11:56:17 2015 -0800

            complete 'hello world' phrase

        commit 230d2a431184c93da0d7a5f7fbeaa00e168e005e
        Author: Erik Eldridge <erik@twitter.com>
        Date:   Sun Feb 15 11:27:45 2015 -0800

            add file

1. Run `git reset` to reset history back to the given commit:

        $ git reset 230d2a431184c93da0d7a5f7fbeaa00e168e005e`

1. Run `git status` to see which files have changed:

        $ git status
        On branch master
        Changes not staged for commit:
          (use "git add <file>..." to update what will be committed)
          (use "git checkout -- <file>..." to discard changes in working directory)
        
          modified:   file.txt
        
        no changes added to commit (use "git add" and/or "git commit -a")

1. Run `git diff` to see the changes:

        $ git diff
        diff --git a/file.txt b/file.txt
        index d72af31..87e05f0 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         asd
        +world

## Reflect

In the steps above, we've started working with git history in a "local" repository.

In the github exercise, we'll start working with a "remote" repository.

## Related

* [git - the simple guide](http://rogerdudler.github.io/git-guide/)
* [Pro Git](http://gitbookio.gitbooks.io/progit/)