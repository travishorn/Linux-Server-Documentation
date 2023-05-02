---
label: HTTPS/SSL on nginx
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -15
---

# HTTPS/SSL on nginx

## Prerequisites

- nginx configured as a reverse proxy for a web application
- a domain name (or scroll down to the "Self-Signed" section below)
- A firewall rule to accept traffic on TCP port 443 has been added
- If using a virtual machine, port 443 is forwarded

## Prepare your nginx Configuration

Edit `/etc/nginx/sites-available/default`

Find the line that starts with `server_name` and replace the `_` character with
the domain name the server will use. You can configure multiple domains by
separating them with a space.

```
server_name example.com www.example.com
```

Save and close the file.

Verify the nginx configuration.

```sh
sudo nginx -t
```

If all is well, reload nginx.

```sh
sudo systemctl reload nginx
```

## Use Certbot to enable HTTPS

Use Certbot if your server is public-facing and you are ready to install a real
certificate signed by [Let's Encrypt](https://letsencrypt.org/) (a Certificate
Authority which issues certificates for free). Skip to the next section if you
just want to use a self-signed certificate.

Install `certbot` and `python3-certbot-nginx`.

```sh
sudo apt update
sudo apt install -y certbot python3-certbot-nginx
```

Generate the SSL certificate.

```sh
sudo certbot --nginx -d example.com -d www.example.com
```

You will be asked some questions, including your email address. Answer the
questions to continue.

Certbot automatically generates the certificate and configures nginx for you. It
also sets a timer that runs periodically that will automatically renew your
certificate before it expires.

With the firewall allowing connections and the certificate installed, you should
be able to visit your domain using `https://` and see your web app. You can also
visit it via `http://` and you should be automatically redirected to the
`https://` URL.

## Self-Signed Certificate

If you aren't ready to use a real certificate signed by an authority, you can
use a self-signed certificate instead. Visitors to the site will receive a
warning about the invalid certificate, but they can bypass it, making this a
good option for testing and development.

Make a directory to store the certificate files.

```sh
sudo mkdir /etc/nginx/ssl
```

Change into that directory.

```sh
cd /etc/nginx/ssl
```

### Generate a Certificate

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

### Configuration

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

You should now be able to visit https://localhost and see your web app. You can
also visit http://localhost and you should be automatically redirected to
https://localhost.

!!!warning
Since you're using a self-signed certificate, your browser will probably warn
you that the certificate is invalid. You can usually bypass this by clicking
**Advanced** and **Proceed** or something similar.
!!!
