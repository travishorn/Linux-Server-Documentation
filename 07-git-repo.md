---
label: Create a New git Repository
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -7
---

# Create a New git Repository

## Prerequisites

- The server has been set up as a git server
- You are logged in as the non-root user

## Create and set permissions

Initialize a bare repository in the home directory.

```sh
git init --bare ~/repos/[repo name]
```

Change `[repo name]` to a name of your choosing. Example: `my-app`

Write a descriptive name or short description of the repository.

```sh
echo "Some short description" > ~/repos/[repo name]/description
```

Now developers who can authenticate as this local user can clone, pull from, and
push to this repository.
