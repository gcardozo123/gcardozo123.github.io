---
layout: post
title: Git Cheat Sheet
tags: [git, github, tips and tricks]
date: 2018-04-30T12:27:19-04:00
---

## You must gather your ~~party~~ aliases before venturing forth
First things first, let's make our lives easier by adding some aliases to git, you can either run these commands


{% highlight powershell %}
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.ci commit
git config --global alias.st status
{% endhighlight %}

Or you can run `git config --global -e` to open your text editor, and add the following to your config file:
{% highlight plaintext %}
[alias]
  co = checkout
  br = branch
  ci = commit 
  st = status
{% endhighlight %}

Now, instead of typing `git status` you can simply type `git st`. Consider the amount of key presses you're going to save.

## Sample config file in case you need one
The following config file contains the `autocrlf` option to properly configure the line-endings, and the option that stores your credentials so you don't need to login everytime you want to push something to your remote.

{% highlight plaintext %}
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
{% endhighlight %}

## Cheat sheet

{% highlight powershell %}
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
{% endhighlight %}

## Bonus

If you're on Windows, consider installing <a href="http://cmder.net" target="_blank">Cmder</a>, it's much better than the Windows command prompt.  

<script type='text/javascript' src='https://ko-fi.com/widgets/widget_2.js'></script><script type='text/javascript'>kofiwidget2.init('Buy me a coffee', '#151a19', 'M4M4NEUK');kofiwidget2.draw();</script> 
