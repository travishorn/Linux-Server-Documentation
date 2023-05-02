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
- A [Next.js](https://nextjs.org/) app has been written and its source code is
  in a git repository accessible by the server
- You are logged in to the server as a non-root user
- If using a virtual machine, port 3000 is forwarded

## Clone the repository

Use `git clone` to clone the repository into the `~/apps` directory.

!!!
The repository could be located anywhere.

If it is hosted on the same server as you're currently using, (for example,
inside the `~/repos` directory), you can clone it by passing `git clone` the
path to the repository.
!!!

```sh
git clone ~/repos/my-app ~/apps/my-app
```

Change into the repository.

```sh
cd ~/apps/my-app
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

Serve the project.

```sh
npm start
```

By default, the app will be accessible on port 3000.

## Test the Application Server

Open a web browser and navigate to http://[your server's IP
address/hostname]:3000. You should see your Next.js app.

!!!
You can navigate to http://localhost:3000 as long as...

1. you're using a virtual machine,
2. are working from the host machine, and
3. you have port 3000 forwarded
!!!

This is a simple setup that isn't fault-tolerant or feasible to manage. You may
want to manage your Node.js processes using a manager like PM2.
