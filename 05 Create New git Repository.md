# Linux Server

## Create a New git Repository

### Prerequisites

- The server has been set up as a git server
- You are logged in as the non-root user

### Create and set permissions

Initialize a bare repository in the home directory.

```sh
git init --bare ~/repos/[repo name]
```

Change `[repo name]` to a name of your choosing. Example: `my-app`

Now developers who have the `git` user's credentials can clone, pull from, and
push to this repository.
