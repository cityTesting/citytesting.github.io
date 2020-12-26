---
layout: post
title: Most important git commands
categories: [git]
---
Next we are going to detail the most common commands when we are using git. There are more, but these are the most useful.

**Checkout**, is used to update the files in the working directory with the version of the branch from the remote repository.
* -b, to create a new local branch.

```
git checkout branchName
git checkout -b newLocalBranchName
```

**Pull**, this commands is used to get changes from the remote branch.
```
git pull
```
**Commit**, is used to save our changes to the local repository.
* -m, to add a message to the commit.

```
git commit
git commit -m "message with information of the changes"
```
**Push**, we use this command to upload the changes from our repository to the remote one.
```
git push
```
**Stash**, the command is used when we want to save the local changes but we don't want to commit or push them to the remote repository.
* list, to list of all the stashed modifications.
* apply, to apply one specific modification.

```
git stash
git stash list
git stash apply stash@{0}
```

**Merge**, with this command we incorporate changes from other branch into the current branch. 
```
git merge otherBranchName
```

**Examples**:

Create a local branch, do some changes, commit and push them to the remote repository. With the last command `git push -u origin newBranchName` we will create the new branch in the remote repository:
```
git checkout -b newBranchName
#here we do the changes that we want to do in the files......
git commit -m "new changes...."
git push -u origin newBranchName
```

Merge master branch into other branch:
```
git checkout master
git pull
git checkout otherBranch
git merge master
git push
```
Sometimes after `git merge master` we get conflicts, in that case we would need to fix them (using IDE for example) and then will need to do `git commit`. 
```
git checkout master
git pull
git checkout otherBranch
git merge master
#here we get and resolve the conflicts...
git commit -m "Resolve conflicts"
git push
```




