---
label: Manage Node.js Processes with PM2
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -11
---

# Manage Node.js Processes with PM2

## Prerequisites

- A Node.js app (Next.js or otherwise) is servable
- The Node.js app is not currently running
- If using a virtual machine, port 3000 is forwarded

## Setup

Install PM2 globally using npm.

```sh
sudo npm install -g pm2
```

Create the file `~/apps/ecosystem.config.js`.

```javascript
module.exports = {
  apps: [{
    name: "myapp",
    cwd: "./myapp",
    script: "npm install && npm run build && npm start",
    watch: true,
    ignore_watch: ["node_modules", "package-lock.json", ".next"],
  }],
};
```

Start the web app process.

```sh
pm2 start ~/apps/ecosystem.config.js
```

Check that the web app is running at http://localhost:3000.

Save the currently running configuration.

```sh
pm2 save
```

View the startup script command.

```sh
pm2 startup
```

That command will output a command that looks something like this:

```sh
sudo env PATH=$PATH:/usr/bin /usr/lib/node_modules/pm2/bin/pm2 startup systemd -u myuser --hp /home/myuser
```

Copy the command. Paste it into your terminal and run it.

PM2 will now automatically start the saved Node.js processes at system startup.
Try restarting the machine to test it.

You will probably want the server serving the web app(s) on port 80 and/or port
443 for HTTP/HTTPS. For that, you will have to set up a reverse proxy.
