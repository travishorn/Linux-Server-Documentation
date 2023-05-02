---
label: Set up a git Server
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -6
---

# Set up a git Server

## Prerequisites

- A working Linux server machine
- You are logged in as a non-root user with sudo privileges

## Install git

If it isn't already installed, install git.

```sh
sudo apt update
sudo apt install -y git
```

## Global configuration

Set a default initial branch name in git

```sh
git config --global init.defaultBranch master
```

Feel free to change the name `master` above to anything you like. While `master`
is common, so is `main`.

Configure git to reconcile divergent branches.

```sh
git config --global pull.rebase false
```

## Create Storage Directory

You'll need a place to store any git repositories. To keep things organized,
create a separate directory for them.

```sh
mkdir ~/repos
```

At this point, the server is set up to store git repositories. Next, you should
start creating new repositories on the server that they can pull from and push
to.
