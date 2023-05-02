---
label: Access a Remote Shell with OpenSSH
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -4
---

# Access a Remote Shell with OpenSSH

## Prerequisites

- Linux is installed and a user(s) has been created
- If using a virtual machine, port 22 is forwarded

## Install OpenSSH on the Server

[OpenSSH](https://www.openssh.com/) server may already be installed on your
server. Get the status of the `sshd` service to check.

```sh
systemctl status sshd
```

If you see output that starts with something like the following, OpenSSH server
is already installed and running. You can safely skip to the next section,
[Install OpenSSH on the Client](#install-openssh-on-the-client).

```
ssh.service - OpenBSD Secure Shell server
     Loaded: loaded (/lib/systemd/system/ssh.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2023-05-02 11:11:20 CDT; 10min ago
```

If instead, you see an error like the following, OpenSSH server is not
installed.

```
Unit sshd.service could not be found.
```

In that case, install OpenSSH server.

```sh
sudo apt update
sudo apt install -y openssh-server
```

## Install OpenSSH on the Client

To connect to your server via OpenSSH, you will need to have the client
installed on the workstation you want to connect *from*.

You may already have it installed. Try running `ssh` from a terminal.

If you see output that starts with something like the following, OpenSSH client
is already installed and you can safely skip to the next section,
[Connect](#connect).

```
usage: ssh [-46AaCfGgKkMNnqsTtVvXxYy] [-B bind_interface]
```

Otherwise, if you receive an error message, you must install OpenSSH client. The
process will depend on your operating system.

+++ Windows

1. Open **Settings** > **Apps** > **Optional Features**
2. Search for **OpenSSH Client**
3. Select **Install**

+++ Linux

Install OpenSSH client using your package manager. For example, if you're using
Debian Linux, run:

```sh
sudo apt update
sudo apt install -y openssh-client
```

+++

## Connect

On your workstation machine (not the server), open up a **Terminal**.

!!!
If you're using a virtual machine, you can also connect to the server from the
host machine.
!!!

Connect to the server via SSH.

```sh
ssh [your username]@[your server's IP address/hostname]
```

Be sure to replace `[your username]` with your actual username on the server and
replace `[your server's IP address/hostname]` with your server's actual IP
address or hostname.

!!!
You may use `localhost` as the hostname if...

1. you're using a virtual machine,
2. you're connecting from the host machine, and
3. you have port 22 forwarded
!!!

!!!warning
If you set up your SSH port to something other than `22`, you must add ` -p
[port number]` to the end of the command.
!!!

If asked if you want to continue connecting, type `yes` and press **Enter**

You will be prompted for your user's password on the server. Enter it and press
**Enter**.

You are now logged in to your server remotely via SSH.

While using password authentication with OpenSSH like this is convenient, you
may consider the added benefits of configuring [public key
authentication](https://www.ssh.com/academy/ssh/public-key-authentication).
