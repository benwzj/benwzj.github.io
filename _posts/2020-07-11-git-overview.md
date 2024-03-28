---
layout: post
title: Git Concepts
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
  - name: GIT Authentication
  - name: Git Workflow
  - name: Git Internals
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
- **the working directory**
- **the staging area**, and 
- **the Git repository**.

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

- Behind the scenes, in the repository's `./.git/objects` directory, Git stores all commits, local and remote. 
- Git keeps remote and local branch commits distinctly separate through the use of branch refs. 
- The refs for local branches are stored in the `./.git/refs/heads/`. Executing the git branch command will output a list of the local branch refs. 
- Remote branches are just like local branches, except they map to commits from somebody else’s repository.

## Credential

### SSH credential storage
- If you use the SSH transport for connecting to remotes, it’s possible for you to have a key without a passphrase, thanks for ssh agent. ssh credential (private key) store in.

- By default, your private and public keys are saved in your ~/.ssh/id_rsa and ~/.ssh/id_rsa.pub files, respectively. you can provide passphrase to protect the private key.

### HTTPS credential storage

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

- That is because, `credential.useHttpPath` is false by default, that means GCM will try use the GitHub credential to connect CodeCommit repo!

## GIT Authentication

SSH and Https are secure way to connect to Git repository.
Git Authenticating to GitHub:
- Https (github CLI, Personal access token)
- SSH

Git Authenticating to CodeCommit:
- Https with Git credential(using Git credential for IAM user)
- git-remote-codecommit tools (using your IAM credential profile)
- Https AWS CLI credential helper(using your IAM credential profile)
- SSH

View your current git remote repo:
`git remote -v`

### Open SSH

- OpenSSH is the most popular suite of secure networking utilities based on the Secure Shell (SSH) protocol. Including the command-line utilities and daemons.
- OpenSSH contains: ssh, ssh-keygen, ssh-add, ssh-agent, sshd, etc.
- Users can create SSH keys using the ssh-keygen command. 
- You key files should store at ~/.ssh

#### How OpenSSH work

- Basic idea is that, ssh run the session process, ssh-agent run as well and check the connections.
- A list of authorized public keys is typically stored in the home directory of the user that is allowed to log in remotely, in the file ~/.ssh/authorized_keys.
- man-in-the-middle attack is a common attack.

#### ssh
- ssh is the SSH client programs.
- ssh run for the duration of a remote login session and are configured to look for the user's private key in a file in the user's home directory (e.g., ~/.ssh/id_rsa).

#### ssh-agent
- ssh-agent runs beyond the duration of a local login session, stores unencrypted keys in memory, and communicates with SSH clients using a Unix domain socket.
- ssh-agent creates a socket and then checks the connections from ssh. 
- When the agent starts, it creates a new directory in /tmp with restrictive permissions. 
- If the ssh-add -c option is set when the keys are imported into the ssh-agent, then the agent requests a confirmation from the user using the program specified by the SSH_ASKPASS environment variable, whenever ssh tries to connect.
- SSH agent forwarding concept
ssh-agents can be "forwarded" onto a server you connect to, making their keys available there as well, for other connections.
- The agent keeps track of user's identity keys and their passphrases. 
- The agent can use the keys to log into other servers without having the user type in a password or passphrase again. 
- This implements a form of single sign-on (SSO).

### ssh for github
 
- When you set up SSH, you will need to generate a new private SSH key and add it to the SSH agent. 
- You must also add the public SSH key to your account on GitHub before you use the key to authenticate or sign commits. 
- You can secure your SSH key by adding your key to the ssh-agent and using a passphrase.
- You are connect with server: git@github.com

#### github supports SSH agent forwarding
- To simplify deploying to a server, you can set up SSH agent forwarding to securely use local SSH keys.
- SSH agent forwarding can be used to make deploying to a server simple. It allows you to use your local SSH keys instead of leaving keys (without passphrases!) sitting on your server.

#### Normal Setup Step
(go to github document for detail)
- Create ssh key:
`$ ssh-keygen -t rsa -b 4096 -C "your_email@example.com"`
(your_email@example.com should be the email for your github account.)

- start a ssh-agent:
`$ eval "$(ssh-agent -s)"`

- Adding your SSH key to the ssh-agent:
`$ ssh-add -K ~/.ssh/id_rsa`

- Adding a new SSH public key to your GitHub account, Go to github and account setting. Add SSH key! 

- Testing SSH connection:
```
$ ssh -T git@github.com
> The authenticity of host 'github.com (IP ADDRESS)' can't be established.
> RSA key fingerprint is SHA256:nThbg6kXUpJWGl7E1IGOCspRomTxdCARLviKw6E5SY8.
> Are you sure you want to continue connecting (yes/no)?
$ yes
```
- Show the remote repo:
```
git remote -v
origin https://github.com/benwzj/animationsApp.git (fetch)
origin https://github.com/benwzj/animationsApp.git (push)
```

- if the repo is not the one you are looking for, then set url: 
```
$ git remote set-url origin git@github.com:USERNAME/REPOSITORY.git
$ git remote -v
origin git@github.com:USERNAME/REPOSITORY.git (fetch)
origin git@github.com:USERNAME/REPOSITORY.git (push)
```
- Now you are supposed to git push


#### Deploy keys
- You can launch projects from a repository on GitHub.com to your server by using a deploy key, which is an SSH key that grants access to a single repository. 
- GitHub attaches the public part of the key directly to your repository instead of a personal account, and the private part of the key remains on your server.

### Https

- GitHub not support username and password on command line any more.
- You can create Personal access tokens. 
- After 2022, you can create Fine-grained personal access tokens! (PAT)
- Once you have a token, you can enter it instead of your password when performing Git operations over HTTPS.
- Currently, fine-grained PATs can only be used against the REST APIs. It will Support for GraphQL soon.

#### Create PATs
- go to your account setting->Delevpe setting, create PAT, setup the permission of the token.

#### Using a personal access token on the command line

- For example, on the command line you would enter the following:
```
$ git clone https://github.com/USERNAME/REPO.git
Username: YOUR_USERNAME
Password: YOUR_TOKEN
```
- Or when you try to git push, it will prompt to input your token.

- Personal access tokens can only be used for HTTPS Git operations, If your repository uses an SSH remote URL, you will need to switch the remote from SSH to HTTPS. Like:
`git remote set-url origin https://github.com/USERNAME/REPOSITORY.git`

- You'll need to update your saved credentials in the git-credential-osxkeychain helper if you change your username, password, or personal access token on GitHub.

### ssh vs https
- SSH being the more secure option and HTTPS easier to use.
- Once you generate the SSH keys, only the machines with the key file on disk can access the repository.

#### The benefits of using SSH for Git are:

- One-time setup. For every action, SSH uses the key file on disk, which is setup when cloning the repository. Although the setup is more complex than HTTPS, it is a one-time effort.
- Improved security. SSH keys are more secure than any password or authentication token, making them almost impossible to crack.
- Time-saving. SSH doesn't require repetitive authentication for every repository action. This feature saves time and makes SSH one of the top reasons for choosing it over HTTPS.

#### The benefits of using HTTPS for Git are:

- Simple setup. HTTPS is easy to set up, requiring only the repo URL and the clone command.
- Availability. HTTPS is available on every operating system and has very few firewall restrictions.
- Portability. Access the repository from any machine by providing the token.
- Https provide more control by Fine-grained personal access tokens.

#### FQA

- Why it need passphrase every time when I connect to SSH git repo.
You can secure your SSH keys and configure an authentication agent so that you won't have to reenter your passphrase every time you use your SSH keys.
You need to setup ssh-agent correctly. 
(A passphrase is similar to a password. However, a password generally refers to something used to authenticate or log into a system. A passphrase generally refers to a secret used to protect an encryption key. )


## Git Workflow

A Git workflow is a recipe or template for how to use Git to accomplish work in a consistent and productive manner. 

- Gitflow workflow used to be popular. But now trunk-based workflows are considered best practices for modern continuous software development and DevOps practices.

### Trunk-based development

Trunk-based development is a version control management practice where developers merge small, frequent updates to a core “trunk” or main branch. Since it streamlines merging and integration phases, it helps achieve CI/CD and increases software delivery and organizational performance.

- With “trunk-based development”, developers regularly merge their code changes into a central repository, ideally several times a day.

### Workflow example

#### Centralized Workflow
Like Subversion, the Centralized Workflow uses a central repository to serve as the single point-of-entry for all changes to the project. 

- A Centralized Workflow is generally better suited for teams migrating from SVN to Git and smaller size teams.

#### Feature Branch Workflow
The core idea behind the Feature Branch Workflow is that all feature development should take place in a dedicated branch instead of the main branch. 

#### Gitflow Workflow

- Gitflow is a legacy Git workflow.
- Gitflow has more, longer-lived branches and larger commits than trunk-based development.

- The git-flow toolset is an actual command line tool that has an installation process.
`$ git flow init`

#### Forking Workflow
The Forking Workflow is fundamentally different than the other workflows. Instead of using a single server-side repository to act as the “central” codebase, it gives every developer a server-side repository. 
This means that each contributor has not one, but two Git repositories: a private local one and a public server-side one. 

- The Forking Workflow is most often seen in public open source projects.

## Git Internals

### Inside the .git directory 

There are four important entries, These are the core parts of Git:
- The **objects directory** stores all the content for your database, 
- the **refs directory** stores pointers into commit objects in that data (branches, tags, remotes and more), 
- the **HEAD file** points to the branch you currently have checked out, 
- and the **index file** is where Git stores your staging area information. 

### Git Objects

- Git is a content-addressable filesystem. It means that at the core of Git is a simple key-value data store. What this means is that you can insert any kind of content into a Git repository, for which Git will hand you back a unique key you can use later to retrieve that content.

There are three object type: 
1. blob, It is most basic storage unit, just like file.
2. tree, It is wrap blobs and other trees, just like folder.
3. commit, It add more info to a tree, including when, who, why. 

#### Understand Blob Object

Two Plumbing Commands:

- `git hash-object` 
this plumbing command takes some data, stores it in your .git/objects directory (the object database), and gives you back the unique key that now refers to that data object.
- `git cat-file`
This command is sort of a Swiss army knife for inspecting Git objects.

Some Example:
```
$ echo 'test content' | git hash-object -w --stdin
d670460b4b4aece5915caf5c68d12f560a9fe3e4
```
then you can :
```
$ git cat-file -p d670460b4b4aece5915caf5c68d12f560a9fe3e4
test content
```
You can also do this with content in files.
```
$ echo 'version 1' > test.txt
$ git hash-object -w test.txt
83baae61804e65cc73a7201a7252750c76066a30
```
At this point, you can delete your local copy of that test.txt file, then use Git to retrieve, from the object database.
```
$ git cat-file -p 83baae61804e65cc73a7201a7252750c76066a30 > test.txt
$ cat test.txt
version 1
```

Check type of this object
```
$ git cat-file -t 83baae61804e65cc73a7201a7252750c76066a30
blob
```

### Tree Object
Tree solves the problem of storing the filename and also allows you to store a group of files together. 
All the content is stored as tree and blob objects, with trees corresponding to UNIX directory entries and blobs corresponding more or less to inodes or file contents. 
{% include figure.html path="assets/img/git-treeobject.png" class="img-fluid rounded z-depth-1" %}

You can use git write-tree to write the staging area out to a tree object.

### Commit Objects

Commit Object wrap tree object and also add information about who saved the snapshots, when they were saved, or why they were saved. 


## Reference

- [git-scm doc](https://git-scm.com/)
- [git-credential-manager doc](https://github.com/git-ecosystem/git-credential-manager/tree/main/docs)
- [cloudbees doc](https://www.cloudbees.com/blog/git-detached-head)
- [git-scm doc](https://git-scm.com/book/en/v2/Git-Tools-Credential-Storage)
