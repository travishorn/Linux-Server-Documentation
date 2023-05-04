---
label: Continuous Integration
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -12
---

# Continuous Integration

## Prerequisites

- A Node.js app (Next.js or otherwise) has its source code hosted in a bare git
  repository on the server (for example, in `~/repos`)
- The app has been cloned to the server (for example, in `~/apps`)
- The app is being run and managed by PM2

## Set up CI

Create a post-receive git hook by creating a file named `~/repos/[repo
name]/hooks/post-receive`.

```sh
#!/bin/bash
cd ~/apps/[app name]
unset GIT_DIR
git pull
npm install
npm run build
pm2 reload [app name]
```

!!!
On the line `pm2 reload [app name]`, the `[app name]` must be the name you set
in `ecosystem.config.js`.
!!!

Make the hook executable.

```sh
chmod +x ~/repos/[repo name]/hooks/post-receive
```

The setup is complete. Here is the process this continuous integration
solution follows:

1. A developer pushes changes to the repository
2. The post-receive hook in the bare repository is automatically executed
3. The hook pulls the changes to the production code in `~/apps`
4. Any missing dependencies are installed
5. The production app is rebuilt
6. The processes are reloaded by PM2, one at a time for zero downtime

All of this happens automatically when changes are pushed to the repository.

For now, the app is accessible directly on port 3000. Ultimately, you will
probably want to serve the Node.js app(s) behind a reverse proxy like
[nginx](https://www.nginx.com/) for improved security, load balancing, caching,
advanced URL rewriting, and simplified deployment.
