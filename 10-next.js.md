---
label: Serve a Next.js App
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -10
---

# Serve a Next.js App

## Prerequisites

- Node.js is installed
- The Next.js app's source code is accessible in a git repository
- If using a virtual machine, port 3000 is forwarded

## Clone the repository

Use `git clone` to clone the repository. The repository could be located
anywhere. If you have it set up on the same server as you're currently using,
inside the `repos` directory, you can clone it by passing `git clone` the path
to the repository.

```sh
git clone ~/repos/myrepo ~/apps/myrepo
```

Change into the repository.

```sh
cd ~/apps/myrepo
```

Install the project's dependencies.

```sh
npm install
```

Disable Next.js telemetry.

```sh
npx next telemetry disable
```

Build the project.

```sh
npm run build
```

Serve the project

```sh
npm start
```

By default, the app will be accessible on port 3000.

## Test the Application Server

Open a web browser and visit http://[your server's IP address/hostname]:3000.

If you're using a virtual machine, are working from the host machine, and you
have port 3000 forwarded, you can visit http://localhost:3000.

!!!
This is a simple setup. You may want to manage your Node.js processes using a
manager like PM2.
!!!
