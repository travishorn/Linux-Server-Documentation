# Linux Server

## Reverse Proxy Using nginx

### Prerequisites

- A running web application on port 3000

### Install nginx

```sh
sudo apt update
sudo apt upgrade
sudo apt install -y nginx
```

#### Forward the Port on VirtualBox

If you are using VirtualBox, you will need to forward port 80.

Open **Oracle VM VirtualBox Manager**.

Click to highlight the server virtual machine.

Click on **Settings**.

Click **Network** in the pane on the left.

Under the **Adapter 1** tab, click **Advanced**.

Click **Port Forwarding**.

Click the **Adds new port forwarding rule.** button on the right. It looks like
a green diamond with a green "plus" sign on top of it.

Under name, enter **HTTP**.

Under **Host Port**, enter **80**.

Under **Guest Port**, enter **80**.

Note that you can change the **Host Port** to another number if you are serving
multiple apps on port **80** on other virtual machines or port 80 is otherwise
already in use on the host machine.

With the port forwarded correctly, you should be able to visit http://localhost
and see the "Welcome to nginx!" page.

### Configure nginx

Edit the nginx configuration file at `/etc/nginx/sites-available/default`.
```

Replace the `location /` block with the following:

```
location / {
    proxy_pass http://localhost:3000;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection 'upgrade';
    proxy_set_header Host $host;
    proxy_cache_bypass $http_upgrade;
}
```

Save and quit editing the file.

Test the new nginx configuration

```sh
sudo nginx -t
```

If successful, restart the nginx service to apply the changes.

```sh
sudo systemctl restart nginx
```

In your web browser, visit http://[server IP address/hostname] (or
http://localhost if using a virtual machine). You should see your Node.js app.

### Block Direct App Access

All requests to the web application should go through port 80 (or 443 if you set
up HTTPS/SSL later). However, the application is still listening on port 3000.
Use a firewall to block external requests to port 3000. See separate guide.

### Take Another Snapshot

If you are setting up the server on a virtual machine, follow these steps to
take another snapshot.

Shut down the machine

```sh
shutdown now
```

In VirtualBox, make sure the machine is selected and then click **Machine** >
**Tools** > **Snapshots** from the menu at the top.

Click the **Take** button.

Under **Snapshot Name**, enter "nginx Installed" and click **Ok**.

Click the **Start** button again. The virtual machine will boot back up.
