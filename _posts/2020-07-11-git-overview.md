---
layout: post
title: Git Overview
date: 2020-07-11
category: Git
tags: Git Overview
toc: 
  - name: What is Git
  - name: Three States
  - name: Git Repository
  - name: Organize commits
  - name: Credential
  - name: What is GCM
  - name: Basic git operations
  - name: Reference
---

## What is Git

Git is a content-addressable filesystem. It means that at the core of Git is a simple key-value data store. What this means is that you can insert any kind of content into a Git repository, for which Git will hand you back a unique key you can use later to retrieve that content.

At a high-level, Git can be thought of as a timeline management utility. git commit is used to create a snapshot of the staged changes along a timeline of a Git projects history.

## Three States

Git has three main states that your files can reside in:
- **Modified** means that you have changed the file but have not committed it to your database yet.
- **Staged** means that you have marked a modified file in its current version to go into your next commit snapshot.
- **Committed** means that the data is safely stored in your local database.

This leads us to the three main sections of a Git project: 
the working tree, the staging area, and the Git directory.
{% include figure.html path="assets/img/git-three-state.png" class="img-fluid rounded z-depth-1" %}

### Staging State

- This is an intermediate area where commits can be formatted and reviewed before completing the commit.

- Staging Area make it possible to quickly stage some of your files and commit them without committing all of the other modified files in your working directory.

- you can just stage the change you need for the current commit and stage the other change for the next commit.

- You can ignore staging area feature, just add a '-a' to your commit command in order to add all changes to all files to the staging area.

### Where do git store staging area

- The management of staging area is based on an object, the blob and the file index. 
- To add content to the staging area, or flag your changes, you use the git add command. This command does 2 things:
1. it creates the blobs corresponding to the new version of the files after modification
2. it feeds the index file which then contains the list of modified files and pointers to the blobs.

## Git Repository

[cloudbees doc](https://www.cloudbees.com/blog/git-detached-head)

### What is Repository in Git
A Git repository is a collection of objects and references. 
Objects(map as circle) have relationships with each other, and references(map as square) point to objects and to other references. 

- The main objects in a Git repository are commits, but other objects include blobs and trees. (So commit is an object.)
- The most important references in Git are branches, which you can think of as labels you put on a commit. (So branch is a reference.)

### What is HEAD

HEAD is another important type of reference. The purpose of HEAD is to keep track of the current point in a Git repo. In other words, HEAD answers the question, “Where am I right now?” (Head is a reference)

### What is 'detached HEAD' state

Normally, or say most of the time, HEAD points to a branch name. We can call this state is attached state. Looks like this:
{% include figure.html path="assets/img/detached-HEAD.png" class="img-fluid rounded z-depth-1" %}

But, if you git checkout `87ec91d`, (`87ec91d` is the hash of a commit), then “You are in ‘detached HEAD’ state[…]”
{% include figure.html path="assets/img/detached-HEAD1.png" class="img-fluid rounded z-depth-1" %}

- detached HEAD state means you are not in a branch, instead, you are in an old commit. 
(in a branch means in the latest commit of a branch)
- detached HEAD is quite useful, allowing you to run experiments that you can then choose to keep or discard.

#### Scenario #1: 
I’ve Made Experimental Changes but I Want to Discard Them. 
Just check out the branch you were in before:
`git checkout <branch-name>`

The changes you made while in the alternate timeline won’t have any impact on your current branch.

#### Scenario #2: 
I’ve Made Experimental Changes and I Want to Keep Them
If you want to keep changes made with a detached HEAD, just create a new branch and switch to it. 

You can create it right after arriving at a detached HEAD or after creating one or more commits. The result is the same. The only restriction is that you should do it before returning to your normal branch.

### What is FETCH_HEAD

FETCH_HEAD is a short-lived ref, to keep track of what has just been fetched from the remote repository. 

## Organize commits

- Behind the scenes, in the repository's ./.git/objects directory, Git stores all commits, local and remote. 
- Git keeps remote and local branch commits distinctly separate through the use of branch refs. 
- The refs for local branches are stored in the ./.git/refs/heads/. Executing the git branch command will output a list of the local branch refs. 
- Remote branches are just like local branches, except they map to commits from somebody else’s repository.

## Credential

### SSH credential storage
- If you use the SSH transport for connecting to remotes, it’s possible for you to have a key without a passphrase, thanks for ssh agent. ssh credential (private key) store in.

- By default, your private and public keys are saved in your ~/.ssh/id_rsa and ~/.ssh/id_rsa.pub files, respectively. you can provide passphrase to protect the private key.

### HTTPS credential storage

[git-scm doc](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)

- It need password for ever connection with HTTP protocols, and it haven’t agent protocol. 
- Now GCM is the out of box tools for credential management.
- All of GCM's configuration settings begin with the term credential.

You can get credential storage info from the configuration file. 
for example (using HTTPS):
- password is stored in a .git-credentials file in the user folder:
credential.helper=store
- password is stored in the windows credential manager:
credential.helper=manager
- macOS Keychain:
credential.helper=osxkeychain
- The “cache” mode keeps credentials in memory for a certain period of time. None of the passwords are ever stored on disk, and they are purged from the cache after 15 minutes. (that means you input password every 15 minutes)
credential.helper=cache

## What is GCM

[git-credential-manager doc](https://github.com/git-ecosystem/git-credential-manager/tree/main/docs)

The goal of Git Credential Manager (GCM) is to make the task of authenticating to your remote Git repositories easy and secure, no matter where your code is stored or how you choose to work. In short, GCM wants to be Git’s universal authentication experience.

- GCM is only useful for HTTP(S)-based remotes. Git supports SSH out-of-the box so you shouldn't need to install anything else.
- GCM maybe initiated by microsoft at the beginning.
- When you choose HTTPS as your preferred protocol for Git operations to Github. You can use GitHub CLI to automatically store your Git credentials for you. 
- Git Credential Manager (GCM) is another way to store your credentials securely and connect to GitHub over HTTPS. 
- With GCM, you don't have to manually create and store a personal access token, as GCM manages authentication on your behalf, including 2FA (two-factor authentication).

- GCM is different software from git. you can install it: 
```
brew install --cask git-credential-manager-core
```

- All of GCM's configuration settings begin with the term credential.

### What is credential.UseHttpPath

- `credential.useHttpPath` Tells Git to pass the entire repository URL, rather than just the hostname, when calling out to a credential provider. (This setting comes from Git itself, not GCM.)
- `credential.useHttpPath` Defaults to false.

#### git config --global credential.useHttpPath true
- This command will instruct git (as well as github) that any credentials used to login should only be associated with the full repository path that was queried, not for the entire domain.
- When you have different repos, servers. set it to true.

#### My experience
- My maxBook have configured to connect GitHub by using https before.
When I tried to git clone a repo from CodeCommit. It had error 403 directly. 
After I executed command: git config --global credential.useHttpPath true
Everything was good then.

- That is because, credential.useHttpPath is false by default, that means GCM will try use the GitHub credential to connect CodeCommit repo!


## Basic git operations

- connect your local repository to your remote repository: 
`git remote add origin https://github.com/benwzj/animationsApp.git`

- push the file to remote repository:
`git push origin master`

- create a branch named my_branch:
`git branch my_branch`

- checkout my_branch, then you switch to my_branch. What you add, commit will be in my_branch:
`git checkout my_branch`

- add file to my_branch, this command just add file to staging area:
`git add file1.txt`
add all untracked files to my_branch: 
`git add -A`
adds all modified and new (untracked) files in the current directory and all subdirectories to the staging area (a.k.a. the index), thus preparing them to be included in the next git commit . 
`git add .`
remove a file form staging area:
`git reset file1.txt`
remove all staging file:
`git reset`

- commit file to repository:
`git commit -m “some information”`

- merge my_branch to master
`git checkout master`
`git merge my_branch`

- Now, you have two branch in you local repository: master and my_branch. If you want to push them to remote repository, then: 
`git push origin master`
or
`git push origin my_branch`

- check log what you have done:
`git log`
Every push have a hash key, Unique hash key.

- if you company have already a remote repo, you can clone that by:
`git clone <url> <where to clone>`

- show remote repo information:
`git remote -v`

- before you push you repo to remote, should pull remote repo to local first:
`git pull origin master`
then
`git push origin master`

- Branch
list all branch in repository:
`git branch`
- list all branch including remote repository:
`git branch -a`
- create my_branch:
`git branch my_branch`
- check out branch, means you are on my_branch now:
`git checkout my_branch`
- check which branch merge: 
`git branch --merged`
- delete branch my_branch:
`git branch -d my_branch`
- delete remote branch:
`git push origin --delete my_branch`

### Programer daily routine: 
```
git branch day_branch
git checkout day_branch
// Do coding …
git status
git add -A
git commit -m “one day work…”
git push -u origin day_branch
git checkout master
git pull origin master
git merge day_branch
git push origin master
```

## Reference

- [git-scm doc](https://git-scm.com/)
- [git-credential-manager doc](https://github.com/git-ecosystem/git-credential-manager/tree/main/docs)
- [cloudbees doc](https://www.cloudbees.com/blog/git-detached-head)