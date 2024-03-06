---
layout: post
title: Most important git commands
categories: [git]
---
Next we are going to detail the most common commands when we are using git. There are more, but these are the most useful.

**Checkout**, It is used to update the files in the working directory with the version of the branch from the remote repository.
* -b, to create a new local branch.

> git checkout branchName  
> git checkout -b newLocalBranchName

**Pull**, this commands is used to get changes from the remote branch.

> git pull

**Commit**, It is used to save our changes to the local repository.
* -m, to add a message to the commit.

> git commit  
> git commit -m "message with information of the changes"

**Push**, we use this command to upload the changes from our repository to the remote one.
* -u, push changes to remote repository. If the branch does not exist remotly, it will create it there.

> git push  
> git push -u origin newLocalBranchName

**Branch**, The branch command is used to list, create or delete branches.
* -a, list all branches (local and remote).
* -d, delete local branch.
* -D, force delete of a local branch.

> git branch  
> git branch -a  
> git branch -d localBranchName  
> git branch -D localBranchName  


**Stash**, the command is used when we want to save the local changes but we don't want to commit or push them to the remote repository.
* list, to list of all the stashed modifications.
* apply, to apply one specific modification.

> git stash  
> git stash list  
> git stash apply stash@{0}  

**Pacth**, sometimes it is interesting or necessary to create patches with changes to be able to pass it on to a colleague without having to commit the changes.
* Difference between our branch and master:


> git diff master Branch1 > ../patchfile  
> git checkout otherBranchName  
> git apply ../patchfile  

* Uncommited local changes:

> git diff --patch > ../patchfile  
> git checkout otherBranchName  
> git apply ../patchfile  

**Merge**, with this command we incorporate changes from other branch into the current branch. 

> git merge otherBranchName


**Examples**:

* Create a local branch, do some changes, commit and push them to the remote repository:

> git checkout -b newBranchName #here we do the changes that we want to do in the files......  
> git commit -m "new changes...."  
> git push -u origin newBranchName  

With the last command  `git push -u origin newBranchName`  we will create the new branch in the remote repository.

* Merge master branch into other branch:

> git checkout master  
> git pull  
> git checkout otherBranch  
> git merge master  
> git push  

Sometimes after  `git merge master`  we get conflicts, in that case we would need to fix them (using IDE for example) and then  `git commit`  is needed. 

> git checkout master  
> git pull  
> git checkout otherBranch  
> git merge master #here we get and resolve the conflicts...  
> git commit -m "Resolve conflicts"  
> git push  

Rebase

> git checkout master  
> git pull  
> git checkout otherBranch  
> git rebase master  
> #fix conflicts with IntelliJ for example  
> git rebase --continue  
> git push --force-with-lease  





