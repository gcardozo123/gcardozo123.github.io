---
layout: post
title: Git Cheat Sheet
tags: [git, github, tips and tricks]
date: 2018-04-30T12:27:19-04:00
---

### You must gather your ~~party~~ aliases before venturing forth
First things first, let's make our lives easier by adding some aliases to git, you can either run these commands


```powershell
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
```

Or you can run `git config --global -e` to open your text editor, and add the following to your config file:
```plaintext
[alias]
  co = checkout
  br = branch
  ci = commit 
  st = status
```

Now, instead of typing `git status` you can simply type `git st`. Consider the amount of key presses you're going to save.

## Sample config file in case you need one
The following config file contains the `autocrlf` option to properly configure the line-endings, and the option that stores your credentials so you don't need to login everytime you want to push something to your remote.

```plaintext
[user]
  name = Your name goes here
  email = your_email@youknow.com
[core]
  autocrlf = true
[credential]
  helper = store
[alias]
  co = checkout
  br = branch
  ci = commit 
  st = status
```

## Cheat sheet

```powershell
# Create branch and change to it: 
git co -b <branch_name>
# Show untracked files: 
git clean -dn
# Delete untracked files: 
git clean -df
# Cherry-pick (it's like merging one commit in your branch): 
git cherry-pick <commit_hash>
# Overwrite the last commit (very useful to change your commit message or
# details in your last commit): 
git commit --amend
#After running amend, you'll need to run:
git push --force
# Delete local branch: 
git branch -d <local_branch>
# Delete branch from the remote:
git push origin --delete <remote_branch>
```