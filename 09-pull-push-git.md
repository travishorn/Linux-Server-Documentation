---
label: Pull from and Push to a git Repository
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -9
---

# Pull from and Push to a git Repository

## Prerequisites

- The server has been set up as a git server
- A bare repository has been created on the server
- You can authenticate as the user where the repository is being hosted

## Working with the Remote Repository

First, clone the repository from the server to your development machine.

```sh
git clone ssh://[username]@[your server's IP address/hostname][path to repository]
```

Example:

```sh
git clone ssh://myuser@myserver/home/myuser/repos/my-app
```

!!!
You can use `localhost` as the hostname as long as...

1. you're using a virtual machine,
2. you're running `git clone` from the host machine, and
3. you have port 22 forwarded
!!!

You will be prompted to enter password for `myuser`.

A new directory named `my-app` (or whatever your repository is named) is created
on your development machine. Change into it.

```sh
cd myrepo
```

You can pull any new changes from the remote repository at any time.

```sh
git pull
```

And you can push your changes to it.

```sh
git push
```

## Public Key Authentication

If you haven't set up public key authentication...

1. you will need to share the password for `myuser` with each developer who
   needs pull/push access.
2. Developers will also need to enter the password each time they pull/push.

Neither of the above conditions is ideal. You can solve both of these issues by
setting up public key authentication.

Make sure you have...

- set up public key authentication on your server
- added your public key to the server user's `authorized_keys` file
- added your server as a configured host on your workstation

The instructions for all of these is in the guide for Public Key Authentication.

[!ref](/05-ssh-keys.md)

With a configured host, users can interact with a server using the alias in the
configuration file.

```sh
git clone ssh://myuser@myserver/home/myuser/repos/myrepo
```

!!!
`myserver` here is the name of the configured host set up in `~/.ssh/config`,
not necessarily the server's hostname (although they can be the same).
!!!

You will be asked for your SSH key passphrase rather than the server user's
password. This eliminates the need to share that password.

If you don't want to enter the SSH key passphrase each time, you can use an SSH
agent (recommended - find more information online) or you can use a key pair
with an empty passphrase (not recommended).

Now that pull and pushing code to the server is possible, you'll probably want
to serve some apps. The next step would be installing the software of your
choice (such as [Node.js](https://nodejs.org/en)) that is required to run any
apps.
