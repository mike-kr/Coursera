# Version Control Review
## What is version control?
**VCS** - Version Control System - Unlike a regular file server which only saves the most recent version of a file, a **version control system** also stores previous versions of each file as you save your changes.  
**Commit** - A VCS even provides a mechanism where the author of the last change (or commit) can record why the change was made.  
**Git**  
**Rollbacks**  
## Version control and Automation
A VCS can function a lot like a time machine, giving you insight into the decisions of the past.  
Whenever you write a commit message after making a change, it's as if the current version of yourself is explaining your decisions to future you, or others who may maintain the codebase.  
Can store config files, etc as well. eg.  
`DNS zone file` specifies the mappings between IP addresses and hostnames in your network.  
If you use good, explanatory commit messages when you update the zone information, you'll have access to **meta information** about the new IP addresses and hostnames present in the zone file.  
# Introduction to Git
## What is Git?
Unlike some version control systems, which are centralized and around a single server, Git has a **distributed architecture**.  
Git *clients* can communicate with Git *servers* using:
* HTTP
* SSH
* Git's special protocol

[Git's architecture or communication protocols](https://git-scm.com/doc)
## What is GitHub?
GitHub is a web-based Git repository hosting service. It adds extra features to the version control functionality of Git like bug tracking, wikis, and task management.  
Alternatives:  
* BitBucket
* Gitlab

[GitHub](https://github.com/)  
[BitBucket](https://bitbucket.org/)  
[Gitlab](https://about.gitlab.com/)
## What does Git do?
Commit - git commit - new snapshot  
`Modified -> Staged -> Committed`  
Modified - changes were made or it's a new file, hasn't been added to snapshot yet because it hasn't been committed to the project  
Staged - files that have been modified or newly added and are marked as ready to be committed to the project. Files that are staged will be part of the next snapshot that you take.
`git status`  
Committed - safely stored snapshot in a database  
Any Git project will have three sections:
* Git directory - You can think of the git directory as the **database** for your Git project, where all of the snapshots you've taken from your commits are stored. When you **clone** a repository, this git directory is copied to your computer. `git init` creates a new repository
* Working tree - a checked-out snapshot of your project. When ready to stage files use the `git add` command
* Staging area - also called an **index**, is a file maintained by Git that has all of the information about what files and changes are going to go into your next commit.

`git commit`  
Git uses an alias called **HEAD** to represent the currently checked out snapshot of your project (like a bookmark where you are)  
`diff`  
`branch`  
`status`  
HEAD -> current branch (e.g., master) -> current commit (aka snapshot)
## Anatomy of Commit Message
What would someone reading the message weeks or months from now want to know about the changes you've made?  
What might be especially important or tricky to understand about them?  
Is there extra information that might help the reader out, like the links to design documents or tickets in your ticketing system?

A commit message is generally broken up into a few sections.  

The first line is a short summary of the commit, followed by a blank line, which is in turn followed by a full description of the changes which details why they are necessary and anything that might be particularly interesting about them or difficult to understand.  

```git
Provide a good commit message example

The purpose of this commit is to provide an example of a hand-crafted, artisional commit message.  The first line is a short, approximately 50 character summary, followed by an empty line.  The subsequent paragraphs are jam-packed with descriptive information about the change, but each line is kept under 72 characters in length.

If even more information is needed to explain the change, more paragraphs can be added after blank lines, with links to issues, tickets, or bugs.  Remember that future you will thank current you for your thoughtfulness and foresight!
# Please enter the commit message for your changes.  Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
# On branch master
# Changes to be committed:
#        new file:    cool_config.txt
#        new file:    super_script.rb
```
`git log` -  will not line wrap  
[Git commit message style Linux kernel documentation](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/Documentation/process/submitting-patches.rst?id=HEAD)  
[Git commit message style impassioned opinions](https://commit.style/)  
[developers](https://robots.thoughtbot.com/5-useful-tips-for-a-better-commit-message)  
[Ruby Style Guide](https://github.com/bbatsov/ruby-style-guide)
## The Basic Git Workflow
`git --version`
`git init` - initialize a git repository  
`git config -l`  
If you add a new file in the directory, like the scripts directory we just mentioned, it starts off as untracked. You can make all kinds of changes to the file, but until you tell Git to track it, Git won't do anything with an untracked file.  
`git add` track files  
Remember that when a file is **staged**, it's been added to the staging area and is ready to be **committed to the git repository.  
Anything that's **unstaged** at this point, like stuff that you haven't run git add on, will not be committed to the repository.  
`git commit`  
Remember that every time you **commit** you take another snapshot, annotated with a commit messsage that you can review later on.
`git status`  
`git log` check history  
`git log --decorate`  
[installing Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)  
[Setting your email in Git](https://help.github.com/articles/setting-your-commit-email-address-in-git/)  
[Keeping your email address private](https://help.github.com/articles/setting-your-commit-email-address-on-github/)  
## The Git and GitHub Workflow
You can follow the workflow at https://github.com/join, which wil set you up with a free account, username and password.  
`git clone https://github.com/dotmatrix337/it-support.git`  
`git push origin master` to send to remote repository  
[GitHub account signup](https://github.com/)
[creating a repo on GitHub](https://help.github.com/articles/create-a-repo/)
# Git Under the Hood
## More with Git
**-a** flag automatically stages every file that is tracked before doing the commit.  
Git commit -a is a shortcut to stage a file and commit it in one step.  Doesn't stage and commit new files.  
**-m** flag lets you inline your commit message, so you don't need to open up a full text editor to type out the description of your change.  
**git rm** command removes files  
**git mv** command moves files  
**.gitignore** file ignores files in repository  
Remember that the "." prefix in a Unix-like filesystem indicates the file or directory is **hidden**, and won't show up when you do a normal directory listing  
[common gitignore configurations](https://gist.github.com/octocat/9257657)  
## Undoing Things
**--amend** option takes anything in staging area and runs the git commit workflow to overwrite the commit  
Try to only use --amend on local repositories. Using **amend** rewrites the git history, removing the previous commit and replacing it with the amended one.  
**git log --stat** prints out extra information  
**git reset** command unstages files you dont want to include
```git
git add *
git status
(use "git reset HEAD <file>..." to unstage)
git reset HEAD bad_file
```
**git checkout** command changes a file back to its earlier committed state
```git
git checkout -- new_script.rb
```
## Rollbacks
**git revert** - In Git, **revert** doesn't just mean undo. Instead, it creates a commit that contains the inverse of the all the changes made in the bad commit, to **cancel them out**.  
`git revert HEAD`  
**-p** flag with git log shows diffs  
** -2** flag tells git we only want to see the last 2 commits  
[Undoing Things](https://git-scm.com/book/en/v2/Git-Basics-Undoing-Things)  
## Identifying a Commit
Commit ID - a Hash calculated using SHA1  
SHA1 - Takes a bunch of data as an input, then produces a 40-character string from that data as an output  
Cryptographic hash functions  
Having consistent data means that what you've got is exactly what you expect.  
> "You can verify the data you get back out is the exact same data you put in." - Linus Torvalds  

The chance of two different commits producing the same hash, which is usually called a **collision**, is really, really small - like, almost impossible.  
If you use a hash to guarantee consistency, you can't change anything in a Git repository without the SHA1 hash changing too.
## Feeling overwhelmed?
Constantly learning new things is one of the awesome benefits of IT since you'll never get bored. So embrace the discomfort! 
It's important to remember that Git is a tool, not a programming language.  
It's always okay to ask for help!  
# Branching and Merging (Advanced Material)
## Introduction
## What is a branch?
Branch
:  A pointer to a particular commit   
A branch represents an independent line of development in a project, and the commit it points to is one in a series of a particular chain of development history.  
Master
: The default branch that Git creates for you when a new repository is initialized  
The master branch is commonly used to represent the *Known-good state*  
## Working with Branches
HEAD - Points to the commit your current working directory is based on, which is usually the same as the branch that you're working on, aka the master  
You can use the git branch command to list, create, delete, and manipulate branches  
**-d** flag deletes branches
To create a branch just specify a name for the branch e.g.:   
`git branch my-new-branch`  
`git checkout awesome-new-feature` to switch to a branch  
`git checkout -b even-better-feature` shortcut to create and switch to a new branch  
This means that when you switch branches in Git, the working directory and commit history will be changed to reflect the snapshot of your project in that branch.  
## Merging
`git merge <branch>` Merges specified branch into *current* branch  
Git used two different algorithms to perform a merge: fast-forward and three-way-merge.  
Fast-forward merge
: This kind of merge happens when all of the commits in the checked-out branch are also in the branch that's being merged  
## Merge Conflicts
**git reset --hard** command  
Using the --hard option can be dangerous, since it will also throw out any uncommitted changes you might have.  
# Git Remotes
## What is a remote?
Remote repository
:   A copy of a project usually hosted on a different computer  
A locally-hosted Git server can run on almost any platform, including Linux, Mac, or Windows, and has benefits like increased **privacy**, **control**, and **customization**. 

If someone has updated the repository since the last time you copied it to your remote branch, Git will tell you that it's time to do an update.  You'll pull down the code, fix and merge conflicts that might arise, and push upstream again.  
## Working with Remotes
`git clone <url>`  
`git remote add origin <url>`  
When you clone a repository using the **git clone** command, Git automatically creates a shortcut to the remote repository called **origin** that points back to the repo you cloned.  
Common ways to reference a remote repository:
* HTTP - usually used tas a way to allow read-only access to a repository
* HTTPS
* SSH  

HTTPS and SSH both provide methods of authenticating users, so you can control who gets permission to push.  
`git remote` - list remotes  
`git remote rename` - rename remotes - `git remote rename origin new-origin`    
`git remote rm` - remove remotes  
`git remote -v` - list remotes and urls  
[git remotes documentation](https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes)
## Git Fetch
`git fetch origin`  
`git fetch origin master`  
Fetched content is downloaded to the remote branches on your repository, where you can run git checkout on it just like a normal branch to have a look.  
`git branch -r` - see remote repos  
Remote branches have a major difference from local branches we've worked with before: they're **read-only**, which means you can only look at them but can't make any changes.  
```git
git fetch origin
git status
git merge origin/master
```
## Git Pull
Running **git pull** will fetch the remote copy of the current branch and automatically try to merge it into the current local branch.  
`git pull origin`  
`git pull origin master`  
## Git Push
`git push origin master`  
`git push --all origin`  - Pushes all of your local branches to the remote repository  
## Git Summary
Remember that version control systems give you access to the history of any scripting project, making fast rollbacks possible and fostering collaboration and accountability.  
# Testing
Software Testing
:    The process of evaluating computer code to determine whether or not it does what you expect  
Formal software testing takes this a step further, codifying tests into its own software and code that can be run to verify that your program does what you expect.  
## Test-Driven Development
When faced with a new problem that can be solved by automation, your gut instinct might be to fire up your text editor and start writing. But, creating some **tests** first makes sure that you've thought through the problem you're trying to solve, and some of the different approaches you might use to accomplish it.  

The test-driven development cycle usually first involves writing a test, then running it to make sure it fails.  
Once you've verified that it fails, you write the code that will satisfy the test, and run the test again.  
If it passes, you can continue on to the next part of your program.  If it fails, you debug it and run the tests again.  
[Ruby-centric take on software testing](https://www.apress.com/us/book/9781484226377?wt_mc=PPC.Google%20AdWords.3.EPR436.GoogleShopping_Product_US)
## Black Box vs White box
White box testing
:    Sometimes called clear box or transparent testing, relies on the test creator's knowledge of the software being tested to construct the test cases.  
Black box testing
:    The tester doesn't know the internals of how the software works  

White box tests are helpful because the test-writer can use their **knowledge** of the source code itself to create tests that cover most of the ways the program behaves.  
Black box tests are useful because they don't rely on knowledge of how the system works, which means their test cases are **less likely to be biased** by the code.  
Unit tests - If the unit tests are created before any code is written based on the specifications of what the code is supposed to do, they can be considered black box unit tests. If unit tests are written alongside or after the code has been developed, and the test cases are constructed with a knowledge of how the software works, they're white-box tests.
## Test Types
Unit tests - verify small isolated parts of a program are correct - Isolation  
Integration tests - verify that actions between different pieces of code in integrated environments are working as expected  
Regression Test - usually part of the debugging and troubleshooting process to verify an issue or error has been fixed once it's been identified.  
Smoke Tests (Build Verification Tests) - Looks for major bugs - sanity checks -  like does the program run.  
Taken together, a group of one or more tests is usually called a *test suite*  
## Unit Tests
Given a known input, does the output of the code match our expectations?  
Because the scope of the test is restricted to a small, specific unit, these types of tests usually run really quickly, and debugging them is simple since there's a **limited number of reasons** for them to fail.  
Besides testing that the code works in the general case, you should also see what happens when you provide it with some input you might not expect it to encounter under normal operations.  
**Edge case**
:    Input to our code that produce unexpected results, and are found at the extreme ends of the ranges of input we imagine our programs will typically work with  
## Writing Unit Tests in Ruby
```ruby
def divider(x, y)
    #Divides two parameters and returns the result.
    return x / y
end
```
Test-unit gem  
Tests should live in a separate file from the code they're testing.
```ruby
require "test/unit"
require_relative "divider"
#White box test
class TestDivider < Test::Unit::TestCase

    def test_basic
        x = 10
        y = 2
        expected_output = 5
        assert_equal(expected_output, divider(x, y))
    end

    def test_divide_by_0
        x = 10
        y = 0
        assert_raise( ZeroDivisionError ) { divider(x, y) }
    end

end
```
When using the test-unit library, you've got to organize your tests into classes that inherit from the TestCase class  
Any methods we define in our TestDivider class will automatically become tests that can be run by the testing framework.  
The **assert_equal** method basically says, "Both of my arguments are equal!"  
**assert_raise** method
Error or exception raised - This means that the normal execution of the program is stopped, and the program will likely crash.  These errors are referred to as runtime exceptions, because they happen when the script is run.  
[test-unit gem documentation](https://rubygems.org/gems/test-unit)
## Summary
Remember that good test help make any automation and scripts you write more robust, resilient, and less buggy.  
When engineers submit their code, it's integrated into the main repository and tests are automatically run against it to spot bugs and errors in a process called **continuous integration**.  
 [Google recommended designing of disaster recovery plans](https://cloud.google.com/solutions/designing-a-disaster-recovery-plan)