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

OpenSSH server may already be installed on your server. Get the status of the
`sshd` service.

```sh
systemctl status sshd
```

If the service isn't recognized, install it.

```sh
sudo apt update
sudo apt install -y openssh-server
```

## Install OpenSSH on the Client

To connect to your server via OpenSSH, you will need to install it on the
machine you want to connect *from*. This will depend on your operating system.

If you're connecting from Windows, it may be installed by default. If not,
follow these steps:

1. Open **Settings** > **Apps** > **Optional Features**
2. Search for **OpenSSH Client**
3. Select **Install**

If you're connecting from Linux, install it using your package manager. For
example, if you're using Debian Linux, run:

```sh
sudo apt update
sudo apt install -y openssh-client
```

If you're connecting from Mac OS, OpenSSH client may already be installed.

## Connect

On your workstation machine (not the server - this could also be the host
machine if you're using a virtual machine), open up a **Terminal**.

Connect to the server via SSH.

```sh
ssh [your username]@[your server's IP address/hostname]
```

Be sure to replace `[your username]` with your actual username on the server and
replace `[your server's IP address/hostname]` with your server's actual IP
address or hostname.

If you're using a virtual machine, you're connecting from the host machine, and
you have port 22 forwarded, you can use `localhost` as your server's hostname.

If you set up your SSH port to something other than `22`, you must add ` -p
[port number]` to the end of the command.

If asked if you want to continue connecting, type `yes` and press **Enter**

You will be prompted for your user's password on the server. Enter it and press
**Enter**.

You are now logged in to your server remotely via SSH.
