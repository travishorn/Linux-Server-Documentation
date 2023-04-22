# Linux Server

## HTTPS/SSL on nginx

### Prerequisites

- nginx configured as a reverse proxy for a web application

### Set up HTTPS/SSL

Make a directory to store the certificate files

```sh
sudo mkdir /etc/nginx/ssl
```

Change into that directory

```sh
cd /etc/nginx/ssl
```

#### Generate a Self-Signed Certificate

If you have an actual certificate you want to use, you can skip this step.

```sh
sudo openssl req -x509 -sha256 -nodes -newkey rsa:2048 -days 365 -keyout localhost.key -out localhost.crt
```

You must answer a few questions about the certificate being generated. Most
important is the common name. If you're using a custom domain name to reference
this server, use that. For example `myserver.dev`. Otherwise, just use something
like `localhost`.

Two new files, `localhost.crt` and `localhost.key` are generated at
`/etc/nginx/ssl`.

#### Configuration

Edit `/etc/nginx/sites-available/default`.

Above the existing `server {` block, add another, new `server {` block like so:

```
server {
    listen 80;
    return 301 https://$host$request_uri;
}
```

In the lower, already-existing server block, remove the first two lines (they
both start with `listen`). Replace them with the following:

```
server {
    listen 443 ssl;
    ssl_certificate /etc/nginx/ssl/localhost.crt;
    ssl_certificate_key /etc/nginx/ssl/localhost.key;

    # ...
}
```

Save and close the file.

Test to make sure the nginx configuration is valid.

```sh
sudo nginx -t
```

If it is, restart the nginx service.

```sh
sudo systemctl restart nginx
```

#### Forward the Port on VirtualBox

If you are using VirtualBox, you will need to forward port 443.

Open **Oracle VM VirtualBox Manager**.

Click to highlight the server virtual machine.

Click on **Settings**.

Click **Network** in the pane on the left.

Under the **Adapter 1** tab, click **Advanced**.

Click **Port Forwarding**.

Click the **Adds new port forwarding rule.** button on the right. It looks like
a green diamond with a green "plus" sign on top of it.

Under name, enter **HTTPS**.

Under **Host Port**, enter **443**.

Under **Guest Port**, enter **443**.

Note that you can change the **Host Port** to another number if you are serving
multiple apps on port 443 on other virtual machines or port 443 is otherwise
already in use on the host machine.

With the port forwarded correctly, you should be able to visit https://localhost
and see your web app. You can also visit http://localhost and you should be
automatically redirected to https://localhost.

Note that if you're using a self-signed certificate, your browser will probably
warn you that the certificate is invalid. You can usually bypass this by
clicking **Advanced** and **Proceed** or something similar.

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

Under **Snapshot Name**, enter "HTTPS/SSL Set Up" and click **Ok**.

Click the **Start** button again. The virtual machine will boot back up.
