# Managing source code

This section is intended to give you experience with three common tools for maintaining a high-quality code base: version control, issue tracking and code review.

Version control helps us make incremental changes to our code. Issue tracking enables our users to provide feedback. Code review improves the quality of code going into our code base.

## Version control

Version control enables us to make incremental changes to our code base. We can then restore previously working versions, compare changes, and recover details about why a change was made.

We can use [git](http://git-scm.com/) and [github](https://github.com/) to explore version control.

### Install git

Git should already be installed if you set up your development environment using this project's Vagrantfile, but in case it's not, run the following in your Ubuntu terminal:

    sudo apt-get install git
    ...
    $ git --version
    git version 2.0.4

### Diving into Git

* `git init` creates a new repository
* `git status` lists files that have changed
* `git add` "stages" files to commit
* `git commit` commits staged changes to a repository
* `git log` lists all commits
* `git show` shows changes in a commit
* `git diff` shows what's changed
* `git reset` "unstages" changes
* `git revert` reverts a commit
* `git branch` encapsulates changes
* `git merge` merges one branch into another
* `git pull` pulls commits from a remote repository
* `git push` pushes commits to a remote repository
* `git clone` makes a local copy of a remote repository

### Create a new repository

1. Create a new directory:

        $ mkdir foo

1. Change into that directory:

        $ cd foo

1. Create a new git repository:

        $ git init

### See which files have changed

    $ git status
    
    On branch master
    
    Initial commit
    
    nothing to commit

### Commit a change to a repository

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
        
        Initial commit
        
        Changes to be committed:
          (use "git rm --cached <file>..." to unstage)
        
        	new file:   file.txt
        ...

1. Finalize the commit.
    
        $ git commit

    Git will launch your default text editor. Write a brief message, eg "Add file", and exit the text editor.
    
    If you haven't already configured git with your email and name, you'll be prompted to do so:

        $ git commit
        
        *** Please tell me who you are.
        
        Run
        
          git config --global user.email "you@example.com"
          git config --global user.name "Your Name"

### See all commits in a repository

    $ git log
    commit c9b26e67839d1e53b017b4a02c55bb8e7f4eefec
    Author: Erik Eldridge <erik@example.com>
    Date:   Fri Mar 13 04:22:00 2015 +0000
    
        Add file

### Show the changes in a commit

    $ git show c9b26e67839d1e53b017b4a02c55bb8e7f4eefec
    commit c9b26e67839d1e53b017b4a02c55bb8e7f4eefec
    Author: Erik Eldridge <erik@example.com>
    Date:   Fri Mar 13 04:22:00 2015 +0000
    
        Add file
    
    diff --git a/file.txt b/file.txt
    new file mode 100644
    index 0000000..45b983b
    --- /dev/null
    +++ b/file.txt
    @@ -0,0 +1 @@
    +hi

### See what's changed

1. Create another change, eg:

        $ echo "world" >> file.txt

1. Run `git diff` to see what has changed:

        $ git diff
        diff --git a/file.txt b/file.txt
        index 45b983b..3b097cd 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         hi
        +world

### Unstage changes

1. Run `git add` to stage the change made above
1. Run `git status` to see what's staged for the next commit
1. Run `git reset` to unstage:

        $ git reset
        Unstaged changes after reset:
        M file.txt

### Revert a commit

1. Add and commit (using the -m shortcut) the change:

        $ git add file.txt
        $ git commit -m "Update file.txt"
        [master 59d700c] Update file.txt
        1 file changed, 1 insertion(+)
        $ git log
        commit 06abe628a703ed148bfcf21c45efdd2283609d40
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:27:42 2015 +0000
        
            Update file.txt
        
        commit c9b26e67839d1e53b017b4a02c55bb8e7f4eefec
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:22:00 2015 +0000
        
            Add file
        ...

1. Run `git revert` to undo the given change:

        $ git revert 06abe628a703ed148bfcf21c45efdd2283609d40
        [master f7d0faa] Revert "Update file.txt"
         1 file changed, 1 deletion(-)

1. Run `git log` to see the reversion:

        $ git log
        commit f7d0faa478b7b2ee19feba9c0d76bebfab37b061
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:32:07 2015 +0000
        
            Revert "Update file.txt"
            
            This reverts commit 06abe628a703ed148bfcf21c45efdd2283609d40.
        
        commit 06abe628a703ed148bfcf21c45efdd2283609d40
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:27:42 2015 +0000
        
            Update file.txt
        
        commit c9b26e67839d1e53b017b4a02c55bb8e7f4eefec
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:22:00 2015 +0000
        
            Add file

### Create a branch

To ensure a repository always contains functional code, we can create a "branch" to encapsulate our work in progress.

By convention, the main branch of a repository is usually called "master".

1. Run `git branch` to see your branches:

        $ git branch
        * master
        
1. Run `git branch` with a branch name to create a new branch:

    $ git branch foo

### Select a branch to work in

1. Run `git checkout` with a branch name to work in that branch:

        $ git checkout foo
        Switched to branch 'foo'

1. Make a change and commit, eg:

        $ echo "a new change" >> file.txt
        $ git add file.txt
        $ git commit -m "Add a new change"

1. Use `git diff` to compare master and your current branch:

        $ git diff master
        diff --git a/file.txt b/file.txt
        index 45b983b..0a978b3 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         hi
        +a new change

1. Use `git log` to see commits on your current branch that aren't on master:

        $ git log master..foo
        commit 9c249cd4035b675df73e92861afb0c1609d2a74b
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:40:05 2015 +0000
        
            Add a new change

### Merge branches

Merge changes from one branch into another using `git merge`.

1. Checkout the branch you'd like to merge changes into, eg master:

        $ git checkout master

1. Use `git merge` to merge changes from another branch:

        $ git merge foo
        Updating f7d0faa..9c249cd
        Fast-forward
         file.txt | 1 +
         1 file changed, 1 insertion(+)

1. Compare the branches and observe there are no differences:

        $ git diff master..foo

### Resolve conflicts

If Git can't merge branches cleanly, it will indicate conflicting lines and ask us to resolve.

1.  Create a change on the master branch and commit, eg

        $ echo "starting text" > file.txt
        $ git add file.txt
        $ git commit -m "Add starting text"

1. Create two new branches:

        $ git branch bar
        $ git branch baz

1. Checkout each branch and make a different change to the same line of the same file:

        $ git checkout bar
        Switched to branch 'bar'
        $ echo "bar text" > file.txt
        $ git add file.txt
        $ git commit -m "Add bar text"
        [bar 3557270] Add bar text
         1 file changed, 1 insertion(+), 1 deletion(-)
        $ git checkout baz
        Switched to branch 'baz'
        $ echo "baz text" > file.txt
        $ git add file.txt
        $ git commit -m "Add baz text"
        [baz e76619d] Add baz text
        1 file changed, 1 insertion(+), 1 deletion(-)

1. Checkout master and merge both branches into it:

        $ git checkout master
        Switched to branch 'master'
        $ git merge bar
        Updating feac207..1f2e058
        Updating 0c4320f..3557270
        Fast-forward
         file.txt | 2 +-
         1 file changed, 1 insertion(+), 1 deletion(-)
        $ git merge baz
        Auto-merging file.txt
        CONFLICT (content): Merge conflict in file.txt
        Automatic merge failed; fix conflicts and then commit the result.

1. View the conflicted state (using Unix's [cat](http://en.wikipedia.org/wiki/Cat_%28Unix%29) command):

        $ git status
        On branch master
        You have unmerged paths.
          (fix conflicts and run "git commit")
        
        Unmerged paths:
          (use "git add <file>..." to mark resolution)
        
        	both modified:      file.txt
        
        no changes added to commit (use "git add" and/or "git commit -a")
        $ cat file.txt 
        <<<<<<< HEAD
        bar text
        =======
        baz text
        >>>>>>> baz

1. Edit the file to remove all but the text you'd like to keep, eg:

        $ vim file.txt
        $ cat file.txt 
        baz text

1. Run `git add` and `git commit` to accept the resolved conflict
1. View the changes in your log (using the -p flag to show the diffs):

        $ git log -p
        commit 7c5d054732577aa89581e505e4468928a1a88e3b
        Merge: 3557270 e76619d
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:52:41 2015 +0000
        
            Merge branch 'baz'
            
            Conflicts:
            	file.txt
        
        commit e76619db788991b25c68a1290b37848bbc692d17
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:46:17 2015 +0000
        
            Add baz text
        
        diff --git a/file.txt b/file.txt
        index d2bc3f2..a67f6b2 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1 @@
        -starting text
        +baz text
        
        commit 355727053e3f85b758c7d3d82f9167e154067476
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:45:26 2015 +0000
        
            Add bar text
        
        diff --git a/file.txt b/file.txt
        index d2bc3f2..132d0f8 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1 @@
        -starting text
        +bar text
        
        commit 0c4320fa52e32c23bde122200737bf8b71769de5
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:43:02 2015 +0000
        
            Add starting text
        
        diff --git a/file.txt b/file.txt
        index 0a978b3..d2bc3f2 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1,2 +1 @@
        -hi
        -a new change
        +starting text
        
        commit 9c249cd4035b675df73e92861afb0c1609d2a74b
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:40:05 2015 +0000
        
            Add a new change
        
        diff --git a/file.txt b/file.txt
        index 45b983b..0a978b3 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         hi
        +a new change
        
        commit f7d0faa478b7b2ee19feba9c0d76bebfab37b061
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:32:07 2015 +0000
        
            Revert "Update file.txt"
            
            This reverts commit 06abe628a703ed148bfcf21c45efdd2283609d40.
        
        diff --git a/file.txt b/file.txt
        index 3b097cd..45b983b 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1,2 +1 @@
         hi
        -world
        
        commit 06abe628a703ed148bfcf21c45efdd2283609d40
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:27:42 2015 +0000
        
            Update file.txt
        
        diff --git a/file.txt b/file.txt
        index 45b983b..3b097cd 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         hi
        +world
        
        commit c9b26e67839d1e53b017b4a02c55bb8e7f4eefec
        Author: Erik Eldridge <erik@example.com>
        Date:   Fri Mar 13 04:22:00 2015 +0000
        
            Add file
        
        diff --git a/file.txt b/file.txt
        new file mode 100644
        index 0000000..45b983b
        --- /dev/null
        +++ b/file.txt
        @@ -0,0 +1 @@
        +hi


### Remote repositories

We now have experience working with a git repository on our local machine, but what happens if our laptop or virtual machine dies, or we want to coordinate with other developers? We can use a "remote" repository, ie a repository hosted on another machine, to avoid catastrophe and facilitate collaboration.

Github is widely used and provides an excellent interface for working with remote repositories. 

Create an account if you don’t have one already. It's free for open source projects.

[Github's set-up documentation](https://help.github.com/articles/set-up-git/) describes how to configure your local environment so you can talk to github.

We'll use SSH to talk with Github, so follow [Github's documentation for setting up SSH](https://help.github.com/articles/generating-ssh-keys/).

Setting this up can be a little tedious, so refer to [Github's SSH support](https://help.github.com/categories/ssh/) documentation if you run into trouble.

Setting up your Ubuntu vm will look like this:

    $ ssh-keygen -t rsa -C "erik@example.com"
    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/vagrant/.ssh/id_rsa): 
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/vagrant/.ssh/id_rsa.
    Your public key has been saved in /home/vagrant/.ssh/id_rsa.pub.
    The key fingerprint is:
    5a:d6:2f:b1:99:5e:c6:41:0f:e2:c1:69:81:61:8e:ca erik@example.com
    The key's randomart image is:
    +--[ RSA 2048]----+
    |        oo.      |
    |       +.. o     |
    |      . . * o    |
    |   . .   + + o   |
    |    E   S + . .  |
    |       +   B .   |
    |      .   = =    |
    |         . +     |
    |          .      |
    +-----------------+
    $ eval "$(ssh-agent -s)"
    Agent pid 2244
    $ ssh-add ~/.ssh/id_rsa
    Identity added: /home/vagrant/.ssh/id_rsa (/home/vagrant/.ssh/id_rsa)
    $ cat ~/.ssh/id_rsa.pub
    ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAAKEDM4noIPCkpjk39KrfSz0y4zFJVsiP7gv0Wazi1+uAP/np0WR82ylWK8TTBY6M6LrcYESkl4T4ssLOdDpUrH6jlvLhbBVYjjwPA+omVniAb2kgXGLFwIVIDTqHjNqNtoBzXEGgN5aU4/1sXvcsuEU5kxRq/1fM9XOxJvniLgcbQ5egD/aa6VYd9zqEekNeNqalg6ehXXiw6tSe3D/lFs2NxQn56tt0DQGZwrmXc36ziFEEM7rpcZX5iPFvDhr/6mp4fU93IXikSiWrVXkIzjVHQi23JkYX17nxIEWVzpRXZT/W9A8fQrZOBILKqyK+mtF+a4BrF0sPdUC8hxvxjHT erik@example.com

Per the SSH instructions linked above, copy the id_rsa.pub contents you just printed into your Github account settings.

Follow [Github's documentation for creating a repository](https://help.github.com/articles/create-a-repo/). Look for "…or push an existing repository from the command line" on the page Github loads after you create a repository, and then tell your local repository where your remote respository is:

    $ git remote add origin git@github.com:erikeldridge/cst-395-example.git

### Pushing changes

1. Use `git push` to push your changes to the remote repository you just created, eg:

        $ git push -u origin master
        Warning: Permanently added the RSA host key for IP address '192.30.252.131' to the list of known hosts.
        Counting objects: 20, done.
        Compressing objects: 100% (8/8), done.
        Writing objects: 100% (20/20), 1.48 KiB | 0 bytes/s, done.
        Total 20 (delta 3), reused 0 (delta 0)
        To git@github.com:erikeldridge/cst-395-example.git
         * [new branch]      master -> master

1. Reload your repository's page on Github
1. Observe you can now see the file in your repository
1. Click the link to your "8 commits"
1. Observe Github provides a web interface to your git history
1. Browse around Github and take a look at other repositories, eg [Android](https://github.com/android), [Linux](https://github.com/torvalds/linux), [Hadoop](https://github.com/apache/hadoop), etc.

After your done browsing, use the review group generator we created [earlier](introduction.md) to generate a group for exercise 1:

    $ java Generator 1 erik@example.com
    [martha@example.com, alex@example.com, erik@example.com]

Send an email to your review group linking to your repository. You should receive similar emails from the other members of your group. If you don’t, reach out and request one.

### Clone repositories

We "clone" a repository to make a copy we can work with locally.

The clone is aware of the repository it was cloned from, so we can push our changes back to it.

Follow [Github's documentation for working with remote repositories](https://help.github.com/articles/fetching-a-remote/), which describes cloning.

Use `git clone` to clone the repositories of your group:

    $ cd ~
    $ git clone https://github.com/<github username>/<repository name>.git

Build and run them. They should generate the same review group your program did.

    $ cd <repository name>
    $ javac Generator.java
    $ java Generator 1 erik@example.com
    [martha@example.com, alex@example.com, erik@example.com]

### Pull changes

To pull changes, we need to create a change somewhere other than our local repo. I'll use Github for this example.

1. In Github, navigate to your project's page
1. Navigate to your file.txt
1. Click the button with the pencil icon to edit the file
1. Make a change and click the "Commit changes" button
1. In your local terminal, run `git pull` to pull these changes into your local repository:

        $ git pull origin master
        remote: Counting objects: 3, done.
        remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
        Unpacking objects: 100% (3/3), done.
        From github.com:erikeldridge/cst-395-example
         * branch            master     -> FETCH_HEAD
           7c5d054..0c3e437  master     -> origin/master
        Updating 7c5d054..0c3e437
        Fast-forward
         file.txt | 1 +
         1 file changed, 1 insertion(+)

### Best-practices

I know we only have a few example commits so far, but keep the following best-practices in mind as we go forward:

* Only commit working code; the master branch should be shippable at all times
* Prefer small commits over monolithic changes
* Do not commit dead code
* Style commit messages according to [git best-practices](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)
* Include [issue tracking](issue_tracking.md) details when available

### Learn more

* [git - the simple guide](http://rogerdudler.github.io/git-guide/)
* [Pro Git](http://git-scm.com/book/en/v2)'s chapter on [Git Basics](http://git-scm.com/book/en/v2/Git-Basics-Getting-a-Git-Repository)
* We're using Github for this book, but [Bitbucket](https://bitbucket.org/) is a comparable product

## Issue tracking

Issue tracking tools provide a structured way to collect feedback.

The nature of the product will determine the appropriate tools to use.

Since we're focused on software development in this book, we'll use Github's issue tracking to explore the concept.

### Creating issues

Identify an issue or enhancement (it can just be a comment) in your group's projects. Did the project build? Was it correct? Was it easy to figure out? Could you understand the code? If the code you’re reviewing doesn’t run, or produces an incorrect result, this is a great opportunity to get it running.

Create an issue, as described in [Github's issue tracker documentation](https://guides.github.com/features/issues/)

Structure your issue report to include:
  * A quick summary
  * What you were trying to do when you noticed the issue
  * Steps to reproduce the issue
  * Error details, like a stacktrace, error message, etc
  * Your environment, eg device type, OS version, app version, etc
  * Let the project owner figure out details like assignee, milestone, labels, etc

### Learn more

* [Github's issue tracker](https://guides.github.com/features/issues/)
* For comparison, [Jira](https://www.atlassian.com/software/jira) is another widely used issue tracker. Bitbucket provides an [issue tracker](https://confluence.atlassian.com/display/BITBUCKET/Use+the+issue+tracker) that's very similar to Github's

Identify an issue or enhancement (it can just be a comment) in each project. Did the project build? Was it correct? Was it easy to figure out? Could you understand the code? 

If the code you’re reviewing doesn’t run, or produces an incorrect result, this is a great opportunity to get it running.

Open an issue on Github for the project (documentation)

## Code review

We can improve the quality of the code base by reviewing changes before they are submitted.

Code reviews also serve to distribute liability, facilitating change.

We'll use Github to explore the idea of code review.

### Create a review

Identify your reviewers using the review group generator for section 1:

    $ java Generator erik@example.com 1
    [joy@example.com, javier@example.com, erik@example.com]

In Github, fork each of your group's repositories.

Clone each forked repo.

In each, create a branch and implement a fix for the issue you reported earlier.

Push the branch to your forked repo.

Create a pull request and mention your reviewers.

### Perform a review

If you are a reviewer, click through the “files changed” tab on the pull request to see the diff.

If you don’t own the repo you’re reviewing code for, just comment on the pull request saying if it looks good or not.

If you own the repo, and the change looks good, merge the change and close the issue.

### Identify reviewers

We can use a tool like our group generator to randomly assign teammates to review groups. Random assignment ensures teammates maintain experience with the entire code base.

Alternatively, we can pre-identify owners of a code base and automatically add them to a review. Github uses "contributors" for this.

Some projects may define a two-step review process: first a member of my team reviews and then an owner approves.

### Review checklist

We can use a checklist to improve consistency in our reviews.

For example:
* Prefer consistency
* Adhere to [style guide](style.md) and [shared principles](../collaboration/principles.md)
* Pay special attention to:
	* security-related changes
	* integrations with remote services
	* importing 3rd-party code
* Localization
* Accessibility
* No dead code
* Design review for significant changes
* Reviewers participate in design review
* Prefer smaller, milestone commits over monolithic changes
* Maintain or improve test coverage
* No surprising changes
* Add recurring issues to this checklist and static analysis tools
* Large changes must run locally

### Learn more

* [Misko Hevery's code reviewers guide](http://misko.hevery.com/code-reviewers-guide/)
* [Github's documentation on forking a respository and submitting a pull request](https://guides.github.com/activities/forking/)

