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

Note that you will need to enter the `myuser` user's password each time. You can
bypass this by setting up SSH keys if desired.