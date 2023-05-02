---
label: Access a Remote Shell with OpenSSH
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -5
---

# SSH Key Authentication

## Prerequisites

- OpenSSH server is running on your server
- OpenSSH client is installed on your workstation

## Prepare the Server

This section needs to be done once per local user account on the server.

Log in to the user account you want to use.

Create a directory called `.ssh` in your home directory.

```sh
mkdir ~/.ssh
```

Set the directory's permissions so only you, the owner can read, write, and
execute it.

```sh
chmod 700 ~/.ssh
```

Create an empty `authorized_keys` file.

```sh
touch ~/.ssh/authorized_keys
```

Set the file's permissions so only you, the owner can read and write it.

```
chmod 600 ~/.ssh/authorized_keys
```

OpenSSH Server will give user account access to anyone who can provide a private
key that matches the public keys stored in that file.

## Generate an SSH Key Pair

Next, create a key pair. The public key will be added to the previously created
`authorized_keys` file.

On your workstation (not the server), open up a **Terminal**.

Create a directory called `.ssh` in your home directory.

```sh
mkdir ~/.ssh
```

Change into the directory.

```sh
cd `/.ssh
```

Generate a key pair.

```sh
ssh-keygen -f [your name]@[location of workstation]-[timestamp] -C [your name]@[location of workstation]-[timestamp]
```

For example:

```sh
ssh-keygen -f myname@work-2023-05-01 -C myname@work-2023-05-01
```

!!!
You can actually replace `[your name]@[location of workstation]-[timestamp]`
with anything you want. But I recommend the above because it will help identify
a key by the name of the person it belongs to, the location it will be used
from, and the date it was created.
!!!

Upon running the command, you will be asked for a passphrase. You can choose
anything you want here, or you can leave it blank. Make sure you understand the
security implications of leaving the passphrase blank before you do it.

Two files are created:

- `myname@work-2023-05-01`
- `myname@work-2023-05-01.pub`

`myname@work-2023-05-01` contains your private key that you will need to provide
any time you try to connect to the server. It is not recommended to copy this
file to another machine. If you need to connect from multiple machines, create
multiple key pairs. Protect this file like a password.

`myname@work-2023-05-01.pub` contains your public key that you can share. Any
user on any device which has this key entered into its `authorized_keys` file
grants you access to the user account (as long as you can provide the private
key).

## Add a Public Key to a User

Once you have a key pair, you'll want to add the public key to a local user
account on the server.

From your workstation, copy the public key into the server user's
`authorized_keys` file.

```sh
cat ~/.ssh/[public key filename].pub | ssh [your username]@[your server's IP address or hostname] "cat >> ~/.ssh/authorized_keys"
```

Be sure to replace `[public key filename]` with the actual public key filename,
`[your username]` with your actual username on the server, and `[your server's
IP address/hostname]` with your server's actual IP address or hostname.

If you're using a virtual machine, you're connecting from the host machine, and
you have port 22 forwarded, you can use `localhost` as your server's hostname.

The command will need the user's current password to log in and copy the public
key.

## Connecting to the Server using an SSH Key

From your workstation, connect to the server, providing the SSH private key for
authentication.

```sh
ssh [your username]@[your server's IP address/hostname] -i ~/.ssh/[private key filename]
```

You will be asked for the key pair's passphrase (if any). Once authenticated,
you will be connected to your server as before.

## Using a Configured Host

The command you must execute to connect to your server via SSH is tedious to
type out. You can alleviate this with a configured host.

On your workstation, create or edit `~/.ssh/config`.

```
Host myserver
  HostName myserver
  User myusername
  Port 22
  IdentityFile ~/.ssh/[private key filename]
```

Make sure to replace `myserver` and `myusername` with the appropriate values.

With that file saved, you can execute the much shorter `ssh` command.

```sh
ssh myserver
```

## Disabling Password Authentication

Once SSH key authentication is set up and working correctly, you may want to
disable the less-secure password authentication.

Edit `/etc/ssh/sshd_config`. Somewhere in the file, add the following line.

```
PasswordAuthentication no
```

Restart the SSH server.

```sh
sudo systemctl restart sshd
```

## Removing Access

If you need to remove access from a particular key pair, edit the server user's
`~/.ssh/authorized_keys` file. and delete the line which has the public key you
want to remove.
