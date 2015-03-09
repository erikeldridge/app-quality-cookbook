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
    commit 230d2a431184c93da0d7a5f7fbeaa00e168e005e
    Author: Erik Eldridge <erik@example.com>
    Date:   Sun Feb 15 11:27:45 2015 -0800
    
        Add file
    ...

### Show the changes in a commit

    $ git show 230d2a431184c93da0d7a5f7fbeaa00e168e005e
    commit 230d2a431184c93da0d7a5f7fbeaa00e168e005e
    Author: Erik Eldridge <erik@example.com>
    Date:   Sun Feb 15 11:27:45 2015 -0800
    
        Add file
    
    diff --git a/file.txt b/file.txt
    new file mode 100644
    index 0000000..d72af31
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
        index d72af31..87e05f0 100644
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

1. Add and commit the change reset above
1. Run `git revert` to undo a given change:

        $ git revert 59d700c1db523b5a7d92161664cd5cdd5d9d477f
        [master 8190a0b] Revert "Update file.txt"
         1 file changed, 1 deletion(-)

1. Run `git log` to see the reversion:

        $ git log
        commit 8190a0b66231ddb06587b55c7cb6ade4c66bc6b3
        Author: Erik Eldridge <erik@example.com>
        Date:   Sun Feb 15 15:23:39 2015 -0800
    
            Revert "Update file"
            
            This reverts commit 59d700c1db523b5a7d92161664cd5cdd5d9d477f.
    
        commit 59d700c1db523b5a7d92161664cd5cdd5d9d477f
        Author: Erik Eldridge <erikeldridge@gmail.com>
        Date:   Sun Feb 15 15:08:32 2015 -0800
    
            Update file
    
        commit b974749b85a95f48fe6615f407a237320a68ed5d
        Author: Erik Eldridge <erik@example.com>
        Date:   Sun Feb 15 15:06:12 2015 -0800
    
            Add file

### Create a branch

To ensure a repository always contains functional code, we can create a "branch" to encapsulate our work in progress.

By convention, the main branch of a repository is usually called "master".

To create a new branch, run `git branch` with a branch name:

    $ git branch foo

### Select a branch to work in

1. Run `git branch` without a branch name to see all branches:

        $ git branch
          foo
        * master

1. Run `git checkout` with a branch name to work in that branch:

        $ git checkout foo
        Switched to branch 'foo'

1. Make a change and commit, eg:

        $ echo "a new change" >> file.txt
        $ git add
        $ git commit -m "Add a new change"

1. Use `git diff` to compare master and your current branch:

        $ git diff master
        diff --git a/file.txt b/file.txt
        index d72af31..c5cb59a 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1,2 @@
         hi
        +a new change

1. Use `git log` to see commits on your current branch that aren't on master:

        $ git log master..foo
        commit cb86977de46709ab389a13741bdf8cd0495782cc
        Author: Erik Eldridge <erik@example.com>
        Date:   Sun Feb 15 15:34:27 2015 -0800
    
            Add a new change

### Merge branches

1. Checkout the branch you'd like to merge changes into, eg master:

        $ git checkout master

1. Use `git merge` to merge changes from another branch:

        $ git merge foo
        Updating 8190a0b..cb86977
        Fast-forward
         file.txt | 1 +
         1 file changed, 1 insertion(+)

1. Compare the branches and observe there are no differences:

        $ git diff master..foo

### Resolve conflicts

If Git can't merge branches cleanly, it will indicate conflicting lines and ask us to resolve.

1. Checkout master, create a change, and commit, eg

        $ git checkout master
        $ echo "starting text" > file.txt
        $ git add file.txt
        $ git commit -m "Add starting text"

1. Create two new branches:

        $ git branch bar
        $ git branch baz

1. Checkout each branch and make a different change to the same line of the same file:

        $ git checkout bar
        $ echo "bar text" > file.txt
        $ git add file.txt
        $ git commit -m "Add bar text"
        $ git checkout baz
        $ echo "baz text" > file.txt
        $ git add file.txt
        $ git commit -m "Add baz text"

1. Checkout master and merge both branches into it:

        $ git checkout master
        $ git merge bar
        Updating feac207..1f2e058
        Fast-forward
         file.txt | 2 +-
         1 file changed, 1 insertion(+), 1 deletion(-)
        $ git merge baz
        Auto-merging file.txt
        CONFLICT (content): Merge conflict in file.txt
        Automatic merge failed; fix conflicts and then commit the result.

1. View the conflicted state:

        $ git status
        On branch master
    
        You have unmerged paths.
          (fix conflicts and run "git commit")
    
        Unmerged paths:
          (use "git add <file>..." to mark resolution)
    
          both modified:   file.txt
    
        no changes added to commit (use "git add" and/or "git commit -a")
        $ cat file.txt 
        <<<<<<< HEAD
        bar text
        =======
        baz text
        >>>>>>> baz

1. Edit the file to remove all but the text you'd like to keep, eg:

        $ cat file.txt 
        baz text

1. Run `git add` and `git commit` to accept the resolved conflict
1. View the changes in your log:

        $ git log -p
        commit 617ad893e32eecd2f4e1de97f9c142fc74b816bf
        Merge: 5a5fa49 33eb8ba
        Author: Erik Eldridge <erik@example.com>
        Date:   Tue Feb 17 05:41:14 2015 +0000
        
            Merge branch 'baz'
            
            Conflicts:
                file.txt
        
        commit 33eb8babdf2ade068c875df8c3c087a4ef0b1203
        Author: Erik Eldridge <erik@example.com>
        Date:   Tue Feb 17 05:36:02 2015 +0000
        
            Add baz text
        
        diff --git a/file.txt b/file.txt
        index d2bc3f2..a67f6b2 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1 @@
        -starting text
        +baz text
        
        commit 5a5fa498fcdef61a0c4943883fbb96ddd3810d6c
        Author: Erik Eldridge <erik@example.com>
        Date:   Tue Feb 17 05:35:44 2015 +0000
        
            Add bar text
        
        diff --git a/file.txt b/file.txt
        index d2bc3f2..132d0f8 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1 +1 @@
        -starting text
        +bar text
        
        commit b4088f901f72ed138a9ad5376634504d9b5687fa
        Author: Erik Eldridge <erik@example.com>
        Date:   Tue Feb 17 05:35:10 2015 +0000
        
            Add starting text
        
        diff --git a/file.txt b/file.txt
        index 0a978b3..d2bc3f2 100644
        --- a/file.txt
        +++ b/file.txt
        @@ -1,2 +1 @@
        -hi
        -a new change
        +starting text
        ...

### Remote repositories

We now have experience working with a git repository on our local machine, but what happens if our local machine dies, or we want to coordinate with other developers? We can use a "remote" repository, ie a repository hosted on another machine, to avoid catastrophe and facilitate collaboration.

Github is widely used and provides an excellent interface for working with remote repositories. 

Create an account if you don’t have one already. It's free for open source projects.

Follow [Github's documentation for creating a repository](https://help.github.com/articles/create-a-repo/)

[Github's set-up documentation](https://help.github.com/articles/set-up-git/) describes how to configure your local environment so you can talk to github.

### Pushing changes

1. Create and commit a change
1. Use `git push` to push these changes to the remote repository you created in the [Github set up](../tools/github.md), eg:

        $ git push origin master
        Counting objects: 3, done.
        Writing objects: 100% (3/3), 215 bytes | 0 bytes/s, done.
        Total 3 (delta 0), reused 0 (delta 0)
        To git@github.com:erikeldridge/app-quality-cookbook.git
         * [new branch]      master -> master

Push your review group generator code to your repository.

Use your review group generator to generate a group.

Send an email to your review group linking to your repository. You should receive similar emails from the other members of your group. If you don’t, reach out and request one.

### Clone repositories

We "clone" a repository to make a copy we can work with locally.

The clone is aware of the repository it was cloned from, so we can push our changes back to it.

Follow [Github's documentation for working with remote repositories](https://help.github.com/articles/fetching-a-remote/), which describes cloning.

Clone the repositories of your group:

    $ cd ~
    $ git clone https://github.com/<github username>/<repository name>.git

Build and run them. They should generate the same review group your program did.

    $ cd <repository name>
    $ javac Main.java
    $ java Main f 1
    [e, c, f]

### Pull changes

To pull changes, we need to create a change somewhere other than our local repo. I'll use Github for this example.

1. In Github, navigate to your project's page, eg https://github.com/erikeldridge/app-quality-cookbook
1. Click through the file tree to an individual file, eg [README.md](https://github.com/erikeldridge/app-quality-cookbook/blob/master/README.md)
1. Click the button with the pencil icon to edit the file
1. Make a change and click the "Commit changes" button
1. In your local terminal, run `git pull` to pull these changes into your local repository:

        $ git pull origin master
        remote: Counting objects: 3, done.
        remote: Total 3 (delta 0), reused 0 (delta 0)
        Unpacking objects: 100% (3/3), done.
        From github.com:erikeldridge/app-quality-cookbook
         * branch            master     -> FETCH_HEAD
           b974749..59d700c  master     -> origin/master
        Updating b974749..59d700c
        Fast-forward
         README.md | 1 +
         1 file changed, 1 insertion(+)

### Best-practices

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

