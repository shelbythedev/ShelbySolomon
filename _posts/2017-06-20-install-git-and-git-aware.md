---
layout: post
title: Install Git & Git Aware Terminal
share: true
comments: true
categories: How-To OSX Git
excerpt_separator: <!-- more -->
---

Installing Git on your Mac is silly easy. I'll walk you through the process in Terminal, as well as guide you through how to install Git Aware Terminal -- an awesome little program that keeps you from having to constantly check your Git status.

<!-- more -->

## Install Homebrew, if you don't already have it
To check and see if you already have Homebrew install, run `brew doctor`.

Homebrew is the most common package manager for Mac. With Homebrew you can easily install frameworks quickly and easily. You'll want to run the command below:
```
  ruby -e "$(curl fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
This may take a while to install, but once it completes, run:
```
  brew doctor
```
You should receive `Your system is ready to brew.`. If you get any errors, you may have non-Homebrew installed packages or other problems.

## Install Git
Now that you have Homebrew set up, all you need to do is run the command:

```
  $ brew install git
```

Once that completes, simply run `git` on the command line, and you should receive something that looks like this:
```
  usage: git [--version] [--help] [-C <path>] [-c name=value]
             [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
             [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
             [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
             <command> [<args>]

  These are common Git commands used in various situations:

  start a working area (see also: git help tutorial)
     clone      Clone a repository into a new directory
     init       Create an empty Git repository or reinitialize an existing one

  work on the current change (see also: git help everyday)
     add        Add file contents to the index
     mv         Move or rename a file, a directory, or a symlink
     reset      Reset current HEAD to the specified state
     rm         Remove files from the working tree and from the index

  examine the history and state (see also: git help revisions)
     bisect     Use binary search to find the commit that introduced a bug
     grep       Print lines matching a pattern
     log        Show commit logs
     show       Show various types of objects
     status     Show the working tree status

  grow, mark and tweak your common history
     branch     List, create, or delete branches
     checkout   Switch branches or restore working tree files
     commit     Record changes to the repository
     diff       Show changes between commits, commit and working tree, etc
     merge      Join two or more development histories together
     rebase     Reapply commits on top of another base tip
     tag        Create, list, delete or verify a tag object signed with GPG

  collaborate (see also: git help workflows)
     fetch      Download objects and refs from another repository
     pull       Fetch from and integrate with another repository or a local branch
     push       Update remote refs along with associated objects

  'git help -a' and 'git help -g' list available subcommands and some
  concept guides. See 'git help <command>' or 'git help <concept>'
  to read about a specific subcommand or concept.
```
If you receive that message, Git is installed, and you're good to go.

## BONUS: Installing Git Aware Terminal
*Caveat: not a sponsored endorsement... I just really love their product!*

My friends over at Zetta have created a beautifully simple little aid to help you track your current Git status. Any time that you're in a Git-initialized directory, Git Aware Terminal keeps tabs on all of your changes, and whether they are untracked, tracked, committed, etc. You don't realize what you're missing until you have it!

![git-aware-terminal untracked](/images/untracked.png)
![git-aware-terminal behind](/images/git-behind.png)

### To install:
Go to your home directory by typing `cd ~`.

Then, run the following command:
```
  $ git clone https://github.com/zeg-io/git-aware-terminal.git
```

Open your bash config file (located at `~/.bash_profile`) -- or create the file if it doesn't exist -- and add the following at the top of the file:
```
  if [ -f ~/git-aware-terminal/git-aware-terminal.bash ]; then
    . ~/git-aware-terminal/git-aware-terminal.bash
  fi
  PROMPT_COMMAND="parse_git_branch"
```
**NOTE: Make sure you don't already have a `PROMPT_COMMAND=` in your file. If you do, add `parse_git_branch;` to it.**

Now, run the following to reload your terminal (or just close and restart Terminal):
```
  $ . ~/.bash profile
```

You should be good to go! Let me know what you think of Git Aware Terminal in the comments below or drop me a line on [Twitter](http://twitter.com/shelbythedev)!

P.S: While you're at it, you should give a shoutout to Zetta Engineering Group ([zeg.io](http://zeg.io))!
