# Intro to Git

Git is a _version control system_. It's primarily useful for allowing multiple developers to work on a piece of code in parallel and to allow developers to track previous versions of their code. 

In this introduction, I will be ignoring many of the quality-of-life features that make Git easier to use. Instead, I aim to introduce the basic commands and how they work.

## Centralized Workflows with Git

Centralized workflows are the simplest way to manage Git repositories. Although Git can operate in a distributed way, many companies adopt centralized workflows for version control. 

Why do you think that is the case?

## Using Git with GitHub

Git is a _distributed system_, where each local instance of a Git repository has a full copy of the code (a _repository_). GitHub acts as a centralized location for developers to integrate their changes, so developers can make sure they are not overwriting other developers' changes. 

`git clone <repo>` is the command used to create a clone, or copy of a repository. This is typically used to point to an existing remote repository (i.e. on GitHub) and make a copy of it locally so that you can make changes to the code in the repository. 

`git fetch` is the command used to see what changes have been made on the remote repository. `git fetch` will NOT integrate the changes on the remote repository into your local repository. 

`git pull` is the command that will integrate or _merge_ changes from the remote repository into your code. 

<fig>

This typically will result in conflicts and force you to resolve _merge conflicts_. Merge conflicts will appear as this when running `git pull`: 

```
Auto-merging file.txt
CONFLICT (add/add): Merge conflicts in file.txt
Automatic merging failed; fix conflicts and then commit the result
```

We cover how to resolve merge conflicts in section **Git Merging**

`git push` is the command used to update the remote repository from code with your local repository. 

<fig>

If someone else has committed to that branch after you last pulled from that branch, you may get the following problem: 

```
error: failed to push some refs to <repo>
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Merge the remote changes (e.g. 'git pull') 
hint: before pushing again
```

This situation is caused by someone else making a `git push` to a branch before you made your `git push` to that same branch. This can be resolved using `git pull`. 


## Git Changes
In Git, changes can exist in four states:

1. Untracked (Git does not see these files)
2. Unmodified (Git sees that files are in this state)
3. Modified (files have been modified locally but cannot be seen by Git)
4. Staged (files are in the staging area)

<fig>

`git status` is the command used to check which (untracked, modified) changes have been made in your local repository. It will return something like:

```
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   ./file.java
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   ./other_file.java
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        ./other_other_file.java
```

This command gives a quick summary of what is currently going on in your branch and helps you assess what changes you have made and what you may need to commit. 

`git add <files>` is the command used to stage changes. This will move changes from (1->4) or (3->4). Files that are untracked cannot be operated on by Git. 

The basis of Git is that it builds a series of snapshots of the code. Git calls each snapshot of the code a _commit_.

`git commit` is the command used to save changes to your local git repository. This will move changes from (4->2). `git commit` only works on changes that have already been staged. The only way to have Git remember your changes is with `git commit`. 

Commits should be paired with a message indicating what the commit has changed. This can be accomplished via `git commit -m <your message>`.

Adding all of your modified files to the staging area can be tedious. Instead, if you only want to add files that are modified (e.g. do not add untracked files), you can use `git commit -a`. 

## Git Branching
Branching refers to a method of organizing code that has changes different from the main line. Branching is typically useful for:

1. Collaboration, since this way multiple developers can work on multiple tasks without stepping on each other's toes
2. Ensuring there is always functional code, since the main branch should only contain code that works and passes tests

`git branch <branch name>` is the command used to create a new branch. You should create a new branch when you are planning on working on a new feature in your code. For example, you could create a new branch for each task in an assignment, allocate each task to a separate developer, and then merge them into main. 

`git checkout <branch name>` is the command used to jump to another branch. This is useful if:
1. you would like to run another version of the code 
2. you would like to make changes on another feature
3. you would like to merge your branch into another branch

## Git Merging
Merging tends to be the most challenging part of learning the Git workflow. Merging happens when you would like to unify work done on separate branches, whether that is explicit (`git merge <branch name>`) or implicit (`git push` and `git pull`). 

`git merge <branch name>` is used when you would like to merge changes from another branch (`<branch name>`) into your branch. 

Merges can fail in two ways:

1. there are uncommitted local changes. Uncommitted local changes are easy to fix in Git. 
    - `git commit` is used when you would like to save your change to Git before the changes from the remote repository come in
    - `git stash` is used when you would like to save your changes locally without affected Git. 
    - `git reset` is used to reset your code and get Git to ignore your changes 
2. there is a merge conflict. Most merges are automatically handled by Git (_fast-forward merge_). Thus, merge conflicts can be avoided if developers make sure not to modify the same file from different branches. However, if two developers change the same lines in a file, Git does not know which change to take and will cause a merge conflict. 
    - Conflicts should be resolved manually using a _merge tool_. These should be built into your IDE. 
    - When merging, make sure that the code you are integrating works as intended after your merge. This comes with practice. You should test your code before finishing a merge to make sure everything works properly. 
    - After completing your merge, don't forget to `git add` the merged files to the staging area so you can commit your changes
    - Merge conflicts will show up like this when you run `git status`: 
```
You have unmerged paths.
    (fix conflicts and run "git commit")

Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)
    
        modified: other_file.java 

Unmerged paths:
    (use "git reset HEAD <file>..." to unstage)
    (use "git add <file>..." to mark resolution)

        both modified: file.java
```

`git status` is a helpful command for resolving merges, as it will describe which files you still need to merge. 

`git push --force` is the command used to force the remote repository to update with the state of your local repository. This will overwrite the remote repository's history with your local repository and skip all merge steps. **AVOID USING THIS UNLESS YOU KNOW WHAT YOU ARE DOING**
