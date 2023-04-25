# Linux Server

## Manage Node.js Processes with pm2

### Prerequisites

- A Node.js app (Next.js or otherwise) is servable
- The Node.js app is not currently running

### Set Up pm2

Install pm2 globally using npm.

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

Check that the web app is running at http://[server IP address/hostname]:3000.

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

pm2 will now automatically start the saved Node.js processes at system startup.
Try restarting the machine to test it.

You will probably want the server serving the web app(s) on port 80 and/or port
443 for HTTP/HTTPS. For that, you will have to set up a reverse proxy.

### Take Another Snapshot

If you are setting up the server on a virtual machine, follow these steps to
take another snapshot.

Shut down the machine.

```sh
shutdown now
```

In VirtualBox, make sure the machine is selected and then click **Machine** >
**Tools** > **Snapshots** from the menu at the top.

Click the **Take** button.

Under **Snapshot Name**, enter "pm2 Installed" and click **Ok**.

Click the **Start** button again. The virtual machine will boot back up.
