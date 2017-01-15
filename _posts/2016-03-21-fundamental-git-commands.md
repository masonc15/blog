---
layout: post
title: Fundamental git commands
date: 2016-03-21 12:31:22.000000000 -03:00
type: post
published: true
status: publish
categories: []
tags:
- git
- work-flow
meta:
  _rest_api_published: '1'
  _rest_api_client_id: "-1"
  _publicize_job_id: '20975045635'
  _oembed_68e26aa40d232ad23d2f390f94406507: "{{unknown}}"
  _oembed_cc9294a121209c077ec1bd8e46c7b646: "{{unknown}}"
author:
  login: tidlost
  email: tl.dr.mfw@gmail.com
  display_name: tidlost
  first_name: ''
  last_name: ''
---

## View changes in a file
To view changes in a file in git you just use the command:

```bash
git diff path/to/file.js
```

It happens that whitespace is showed. Which shows almost everything as changed. To solve this, add the -b flag.

```
git diff -b path/to/file
```

## Discard changes in an unstaged file

Every now and then Git shows that I have edited a file, but when I run git diff I can see that I have added and removed the same line. I think this happens if I add a line and then remove it. So basically nothing has changed (and I haven't actually edited the file, just opened it). I think it is better to discard the h2>changes to that the commit does not get cluttered with files that we actually haven't modified. So in order to discard the changes from git I use the following command:

```
git checkout -- app/config/rest.js
```

This is the recommended command from git, and it shows when we run git status.

## Create new branch
There are two ways.

```bash
#Create a branch
git branch nameOfBranch
#Create a branch and switch to it
git checkout -b nameOfBranch
```

## Remove branch
If the branch is merged you can use the command:

```bash
git branch -d myBranch
```

If it is not merged you have to use:

```bash
git branch -D myBranch
```

## Remove remote branch

```
git push origin --delete the_remote_branch
```

## Switch branch without committing

So if you have made some changes in a branch and then switch branch without committing the changes git will automatically merge those branches. Which might not be what you want. But you might not feel ready to commit the changes.
**So how do you switch branch without having to merge the branches and without having to commit the changes?**
The answer is: `stash`.
So, let's say you have made changes that you haven't committed or added but want to switch branch. You just use the following command:

```bash
# Branch: testBranch
git stash
git checkout master
# Make some changes or whatever you wanted to do
git checkout testBranch
git stash pop
```

And now you are back and can keep working on your branch.
Check out this for a better understanding of how to use git stash:
https://git-scm.com/book/en/v2/Git-Tools-Stashing-and-Cleaning

## Create a new remote branch
So let's say that you have a master-branch and you want to create a development-branch.
First you create your branch locally

```bash
git checkout -b development
```

Then you push it to your remote repo

```bash
git push -u origin development
```

## View commit log
So let's say you have made some commits. And then you want to see what those commits were. You can't run `git diff`. So instead you can run `git log -p`
This will show you the difference in each commit.
