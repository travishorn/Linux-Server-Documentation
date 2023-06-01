---
label: Install Node.js
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -10
---

# Install Node.js

## Prerequisites

- A working Linux server machine
- You are logged in as a non-root user with sudo privileges

## Installation

Binary distributions of [Node.js](https://nodejs.org/en) are provided by
[NodeSource](https://nodesource.com/). Installing Node.js on Debian Linux via
the NodeSource distributions is an officially supported method.

First, install curl.

```sh
sudo apt update
sudo apt install -y curl
```

Assume the `root` user.

```sh
sudo -s
```

Install the latest LTS Node.js release via NodeSource.

```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && apt-get install -y nodejs
```

!!!
The above command(s) will install Node.js version 18.x. There may be a more
recent LTS release. You can find out at https://nodejs.org.
!!!

Exit the `root` user shell.

```sh
exit
```

Make sure Node.js is installed correctly by getting its version number.

```sh
node -v
```

It should display something like:

```sh
v18.16.0
```

!!!
The exact version number might vary depending on when you are reading this
guide.
!!!

## Create an App Directory

To stay organized, I recommend creating a directory to store any apps you might
deploy.

```sh
mkdir ~/apps
```

You are now ready to run and serve Node.js apps. You may choose to write and/or
run completely custom Node.js apps, or you may choose to go with a framework like Next.js.
