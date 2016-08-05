---
layout: post
title: Git revert
---

## Revert

Imagine that you write some really crappy code, that is basically just a bug. You can very easily just remove that whole commit without affecting the rest of the code. This can be done by using `git revert`

So the flow is like this

First find the commit hash by running:
```
git log
```

Now that you have the commit you can check out the state of the code at that commit by doing:

```
git checkout 882053469e845f8a67a50a966eaaa42649cb7c97
```

So you realize that the code is shit and you want to take it out from the project, you do:

```
git revert 882053469e845f8a67a50a966eaaa42649cb7c97
```

The commit is still in the project history. Instead it has added a commit that removes the other commit.

So if you then want to revert the revert you just reverted you can just revert it. Höhö.

```
git log
```

Outputs
```
commit 11f43361ef9c6cafc55ed0a43822b08beb1ccc23
Date:   Fri Aug 5 16:07:08 2016

    Revert "Revert "adding""

    This reverts commit a7bce22dc6df73e3307795f3b3ba5fbbe6da9ce8.
```

So you just take that hash and revert it

```
git revert 11f43361ef9c6cafc55ed0a43822b08beb1ccc23
```
