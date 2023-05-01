# Linux Server

## Continuous Integration

### Prerequisites

- A Node.js app (Next.js or otherwise) is being managed by pm2

### Set up CI

Create a post-receive git hook by creating a file named `~/repos/[repo
name]/hooks/post-receive`.

```sh
#!/bin/bash
cd ~/apps/[app name]
unset GIT_DIR
git pull
```

Make the hook executable.

```sh
chmod +x ~/repos/[repo name]/hooks/post-receive
```

Every time changes are pushed to the repository, the post-receive hook will
execute, pulling in the new changes to the running application. Once the changes
are pulled in, pm2 will detect the file changes, rebuild the app, and restart
the server.
