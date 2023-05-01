# Linux Server

## Set up a git Server

### Prerequisites

- A working Linux server machine
- You are logged in as a non-root user with sudo privileges

### Install git

If it isn't already installed, install git.

```sh
sudo apt update
sudo apt install -y git
```

### Global configuration

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

### Create Storage Directory

You'll need a place to store any git repositories. To keep things organized,
create a separate directory for them.

```sh
mkdir ~/repos
```

### Take Another Snapshot

If you are setting up the server on a virtual machine, follow these steps to
take another snapshot.

Shut down the machine

```sh
shutdown now
```

In VirtualBox, make sure the machine is selected and then click **Machine** >
**Tools** > **Snapshots** from the menu at the top.

Click the **Take** button.

Under **Snapshot Name**, enter "git Server Installed" and click **Ok**.

Click the **Start** button again. The virtual machine will boot back up.

At this point, the server is set up to store git repositories. Next, you should
start creating new repositories on the server that they can pull from and push
to.
