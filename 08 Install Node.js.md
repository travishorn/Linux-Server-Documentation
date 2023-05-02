---
label: Install Node.js
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -8
---

# Install Node.js

In order to install Node.js, first install curl.

```sh
sudo apt update
sudo apt install -y curl
```

Assume the `root` user

```sh
sudo -s
```

Install the latest LTS Node.js release

```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && apt-get install -y nodejs
```

!!!
The above command(s) will install version 18.x. There may be a more recent LTS
release. You can find out at https://nodejs.org.
!!!

Exit the `root` user shell

```sh
exit
```

Make sure Node.js is installed correctly by getting its version number

```sh
node -v
```

It should display something like:

```sh
v18.16.0
```

The exact version number might vary.

## Create an App Directory

To stay organized, I recommend creating a directory to store any apps you might
deploy.

```sh
mkdir ~/apps
```

You are now ready to run and serve Node.js web apps.
