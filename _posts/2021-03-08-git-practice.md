---
layout: post
title: Using Git Practice
date: 2021-03-08
category: Git
tags: Git
toc: 
  - name: Classice Case
  - name: Setting up a repo
  - name: Working with remote repo
  - name: Saving changes
  - name: Undoing Changes
---

Git support Parallel Development. 
Parallel Development means you can create branch.
{% include figure.html path="assets/img/git-parallel.png" class="img-fluid rounded z-depth-1" %}

## Classice Case
Pushing an existing unversioned project to a Git repository.

1. Inside project root folder, run `git init` creates a .git environment.
It will create .git folder and .gitignore file.

2. create repository in Git server, e.g. Github.

3. connect the project to Git server Repository.
`git remote add origin git@github.com:benwzj/CodeDeployBlog.git`

4. then you can work on it
`git branch -M main`
`git push -u origin main`


## Setting up a repo

### git init
The `git init` command creates a new Git repository. It can be used to convert an existing, unversioned project to a Git repository or initialize a new, empty repository.

> Executing `git init` creates a .git subdirectory in the current working directory, which contains all of the necessary Git metadata for the new repository. 

#### git init vs. git clone

- At a high level, they can both be used to "initialize a new git repository." 
- However, `git clone` is dependent on `git init`. `git clone` is used to create a copy of an existing repository. 
- Internally, `git clone` first calls `git init` to create a new repository. It then copies the data from the existing repository, and checks out a new set of working files.

#### Bare repositories 
`git init --bare`

- The --bare flag creates a repository that doesn’t have a working directory, making it impossible to edit files and commit changes in that repository. 
- For example, the bare version of a repository called my-project should be stored in a directory called my-project.git.
- Central repositories should always be created as bare repositories, and developers local repositories are non-bare.

#### git init template
- Templates allow you to initialize a new repository with a predefined .git subdirectory. 

### git clone

- git clone is a Git command line utility which is used to target an existing repository and create a clone, or copy of the target repository. 

- git clone is used to create a copy of a target repo, the target repo can be local or remote.
- Git supports a few network protocols to connect to remote repos: ssh, git, http[s].


### git config

Executing git config will modify a configuration text file values on a global or local project level. 

#### Usage
- The most basic use case for git config is to invoke it with a configuration name, which will display the set value at that name. 
- Configuration names are dot delimited strings composed of a 'section' and a 'key' based on their hierarchy. 
- Example:
`git config --global credential.UseHttpPath true`

- `git config --local`
By default, git config use local level:
`.git/config`

- `git config --global`
global flag: 
`~ /.gitconfig`

- `git config --system`
`$(prefix)/etc/gitconfig`

- `git config --list`
list the content of the configuration file
for global file: `git config --list --global`

- `git config --global --edit`
edit the config file

#### content of configuration file
```
-- credential
-- user
-- editor - core.editor
-- Merge tools
-- Colored outputs
-- Formatting & whitespace
```
- Aliases : `git config --global alias.ci commit`
Git aliases allow you to create shortcuts for frequently used Git operations. 
The term alias is synonymous with a shortcut.
Aliases are used to create shorter commands that map to longer commands. 
no direct git alias, but you can: `git config --global alias.co checkout`

## Working with remote repo

### Creating remote repositories

- `git remote add origin <REMOTE_URL>`
This associates the name origin with the REMOTE_URL.

- URL like these:
An HTTPS URL like: https://github.com/user/repo.git
An SSH URL, like: git@github.com:user/repo.git 
An SSH URL, also like: ssh://git-codecommit.us-east-1.amazonaws.com/v1/repos/pipeline-demo

- create a new repository on the command line
```
echo "# CodeDeployBlog" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:benwzj/CodeDeployBlog.git
git push -u origin main
```

- push an existing repository from the command line
```
git remote add origin git@github.com:benwzj/CodeDeployBlog.git
git branch -M main
git push -u origin main
```

### Cloning a remote repo
Just do this:
`git clone git@github.com:benwzj/CodeDeployBlog.git`
Everything is done.

### Changing a remote repository's URL
The git remote set-url command changes an existing remote repository URL.

### Renaming a remote repository
Use the git remote rename command to rename an existing remote.

### Removing a remote repository
Use the git remote rm command to remove a remote URL from your repository.

## Saving changes

`git add git commit git diff git stash .gitignore`

### Git add

- The git add command adds a change in the working directory to the staging area. And this 'stage' changes will be stored in a commit.
- However, git add doesn't really affect the repository in any significant way—changes are not actually recorded until you run git commit.

#### Staging area

- The staging area is considered one of the "three trees" of Git, along with, the working directory, and the commit history.
- the stage lets you group related changes into highly focused snapshots before actually committing it to the project history

#### `git add -p`
Begin an interactive staging session that lets you choose portions of a file to add to the next commit. 

### Git commit
The git commit command captures a snapshot of the project's currently staged changes. 

How it work
- At a high-level, Git can be thought of as a timeline management utility. 
- git commit is used to create a snapshot of the staged changes along a timeline of a Git projects history.
- Commits can be thought of as snapshots along the timeline of a Git project.
- Commits are created with the git commit command to capture the state of a project at that point in time. 

- Git’s version control model is based on snapshots. 
- Git records the entire contents of each file in every commit.

### Git diff

- Diffing is a function that takes two input data sets and outputs the changes between them.
- git diff is a multi-use Git command that when executed runs a diff function on Git data sources, like commits, branches, files.
- The git diff command is often used along with git status and git log to analyze the current state of a Git repo.

How to use git diff

By default git diff will show you any uncommitted changes since the last commit.

- Comparing all changes
Invoking git diff without a file path will compare changes across the entire repository. 

- Comparing files: 
By default git diff will execute the comparison against HEAD. So
`git diff HEAD ./path/to/file`
equal to: 
`git diff ./path/to/file`
(filename is case sensitive.)

- When git diff is invoked with the `--cached` option the diff will compare the staged changes with the local repository. The `--cached` option is synonymous with `--staged`.

- Comparing files between two different commits
`git diff` can be passed Git refs to commits to diff.
`git diff c304ff4 8836b44`

- Comparing branches
`git diff branch1..branch2`

- Comparing files from two branches
`git diff main new_branch ./diff_test.txt`

### git stash 

- git stash temporarily stashes changes you've made to your working copy so you can work on something else, and then come back and re-apply them later on.

- You can reapply previously stashed changes with git stash pop. Popping your stash removes the changes from your stash and reapplies them to your working copy.

### .gitignore

Git sees every file in your working copy as one of three things:
- tracked - a file which has been previously staged or committed;
- untracked - a file which has not been staged or committed; or
- ignored - a file which Git has been explicitly told to ignore.

## Undoing Changes

`git checkout git clean git revert git reset git rm`

### Reviewing old commit

- We use `git log` to get a list of the latest commits. `git log --branches=*` get the commits for specific branch. `git log --oneline` can be a easier way.
- When you have found a commit reference to the point in history you want to visit, you can utilize the `git checkout` command to visit that commit. 
- `Git checkout` is an easy way to “load” any of these saved snapshots onto your development machine.
{% include figure.html path="assets/img/git-reviewcommit.png" class="img-fluid rounded z-depth-1" %}

### Undo commit with `git checkout`

Detached HEAD state
- When you check out a previous commit, for example: `git checkout a1e8fb5`. And then HEAD no longer points to a branch—it points directly to a commit. This is called a “detached HEAD” state. 
- In a detached state, any new commits you make will be orphaned when you change branches back to an established branch. So don’t commit in Detached HEAD state!
- To prevent orphaned commits from being garbage collected, we need to ensure we are on a branch:
From the detached HEAD state, we can execute git checkout -b new_branch. This will create a new branch named new_branch and switch to that state. 
- At this point, we can continue work on this new branch in which the `872fa7e` commit no longer exists and consider it 'undone'.
- Sometime, this is not a appropriate way, for example for public shared repositories. But You can use git revert

- NOTE: Checking out an old file does not move the HEAD pointer. It remains on the same branch and same commit, avoiding a 'detached head' state. You can then commit the old version of the file in a new snapshot as you would any other changes. So, in effect, this usage of git checkout on a file, serves as a way to revert back to an old version of an individual file.

### undo a commit with `git revert`

What is `git revert`

- git revert is used to record some new commits to reverse the effect of some earlier commits (often only a faulty one). If you want to throw away all uncommitted changes in your working directory, you should see git-reset.
- git revert figures out how to invert the changes introduced by the commit and appends a new commit with the resulting inverse content.
- Reverting should be used when you want to apply the inverse of a commit from your project history. 
- if you’re tracking down a bug and find that it was introduced by a single commit. Instead of manually going in, fixing it, and committing a new snapshot, you can use git revert to automatically do all of this for you.

- It's important to understand that git revert undoes a single commit—it does not "revert" back to the previous state of a project by removing all subsequent commits. git reset do that!

- git revert prevents Git from losing history.
- git revert is the ideal 'undo' method for working with public shared repositories.

#### Usage

- Git revert expects a commit ref was passed in.
- If we pass HEAD in: git revert HEAD, Git will create a new commit with the inverse of the latest commit. 
- This adds a new commit to the current branch history and now makes it look like:
{% include figure.html path="assets/img/git-reverse.png" class="img-fluid rounded z-depth-1" %}
- At this point, we have again technically 'undone' the 872fa7e commit.

### undo a commit with `git reset`

- `git reset` - Reset current HEAD to the specified state.

- `git reset` is a complex and versatile tool for undoing changes.
- It has three primary forms of invocation. These forms correspond to command line arguments --soft, --mixed, --hard. 
- The three arguments each correspond to Git's three internal state management mechanism's, The Commit Tree (HEAD), The Staging Index, and The Working Directory.

#### Usage

- If we invoke git reset --hard a1e8fb5 for this example, the commit history is reset to that specified commit.
{% include figure.html path="assets/img/git-reset.png" class="img-fluid rounded z-depth-1" %}
- The log output shows the e2f9a78 and 872fa7e commits no longer exist in the commit history.
- This method of undoing changes has the cleanest effect on history, comparing git revert.
- But git revert method is not good for shared remote repository. Git reset should generally be considered a 'local' undo method. 

### Undoing the last commit

git commit --amend. This will have Git open the configured system editor and let you modify the last commit message. The new changes will be added to the amended commit.

### Git clean

- `git clean` is a convenience method for deleting untracked files in a repo's working directory. 
- `git clean` will make a hard filesystem deletion. Make sure you really want to delete the untracked files before you run it.
- `git clean` is to some extent an 'undo' command. `git clean` can be considered complementary to other commands like g`it reset` and `git checkout` to fully undo any additions and commits in a repository. 

#### option
`git clean -n` will perform a “dry run”. 
`git clean -f` The force option initiates the actual deletion of untracked files from the current directory.

### Conclusion
- get information:
`git status, git log`
- undo local unstaged changes: 
`git restore filename`
- undo local staged changes:
`git restore --staged filename`
- undo latest local commit, and still keep the changes:
`git reset --soft HEAD~`
- undo public committed changes. Following command will undo the changes by creating a new commit and reverting that file to its previous state, as if it never changed:
`git revert cc3bbf7 --no-edit`

