---
layout: post
title: Most important git commands
categories: [git]
---

**Checkout**, is used to updates the files in the working directory with the version of the branch in the remote repository.
* -b, to create a new local branch.

```
git checkout branchName
git checkout -b newLocalBranchName
```

**Pull**, this commands is used to get changes from a remote repository into ours.
```
git pull
```
**Commit**, the command is used to save our changes to the local repository,
* -m, to add a message to the commit.

```
git commit
git commit -m "message with information of the changes"
```
**Push**, we use this command to upload from our repository content to a remote repository.
```
git push
```
**Stash**, the command is used when we want to save the local changes but we don't want to commit or push them.
* list, to list of all the stashed modifications.
* apply, to apply one specific modification.

```
git stash
git stash list
git stash apply stash@{0}
```

**Examples**:


