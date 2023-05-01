# Linux Server

## Pull from and Push to a git Repository

### Prerequisites

- The server has been set up as a git server
- A bare repository has been created on the server

### Working with the Remote Repository

First, clone the repository on your development machine

```sh
git clone ssh://[username]@[your server's IP address/hostname][path to repository]
```

Example:

```sh
git clone ssh://myuser@myserver/home/myuser/repos/myrepo
```

Note: If you're using a virtual machine, you're cloning from the host machine,
and you have port 22 forwarded, you can use `localhost` as the hostname.

You will be prompted to enter the `myuser` user's password.

A new directory named `myrepo` (or whatever your repository is named) is created
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

### SSH Key Authentication

Note that you will need to share the `myuser` password with each developer who
needs pull/push access. Developers will also need to enter the `myuser` user's
password each time they pull/push. This is not ideal. You can bypass this by
setting up SSH keys if desired.

Make sure you have...

- set up SSH key authentication on your server
- added your public key to the server user's `authorized_keys` file
- added your server as a configured host on your workstation

All of those tasks can be found in the guide [SSH Key
Authentication](./05%20SSH%20Key%20Authentication.md).

When all of the above is set up, you can clone the repository.

```sh
git clone ssh://myuser@myserver/home/myuser/repos/myrepo
```

Note that `myserver` here is the name of the configured host set up in
`~/.ssh/config`, not necessarily the server's hostname (although they can be the
same).

You will be asked for your SSH key passphrase (if any) rather than the server
user's password. This eliminates the need to share that password.

If you don't want to enter the SSH key passphrase each time, you can use an SSH
agent (recommended - find more information online) or you can use a key pair
with an empty passphrase (not recommended).
