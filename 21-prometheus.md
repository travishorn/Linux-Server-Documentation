---
label: Monitoring with Prometheus
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -21
---

# Monitoring with Prometheus

## Prerequisites

- A firewall rule to accept traffic on TCP port 9090 has been added
- If using a virtual machine, port 9090 is forwarded

## Installation

Go to https://prometheus.io/download/.

Under **Operating System**, choose **linux**.

Under **Architecture**, choose **amb64**.

In the **prometheus** section, find the version that is labeled **LTS**.

Copy the listed URL. It will be labeled something like
**prometheus-2.37.6.linux-amd64.tar.gz**.

In your terminal, download the file at the copied URL.

```sh
wget https://github.com/prometheus/prometheus/releases/download/v2.37.6/prometheus-2.37.6.linux-amd64.tar.gz
```

!!!
The URL in the line above is just an example, use the actual URL you copied
earlier.
!!!

The file will be downloaded to your machine. Unzip it.

```sh
tar xvfz prometheus-2.37.6.linux-amd64.tar.gz
```

To stay organized, remove the compressed file.

```sh
rm prometheus-2.37.6.linux-amd64.tar.gz
```

Move all of the Prometheus files to `/opt/prometheus`.

```sh
sudo mv prometheus-2.37.6.linux-amd64 /opt/prometheus
```

Create a new user that will run the Prometheus daemon.

```sh
sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus
```

Set the new user as the owner of `/opt/prometheus`.

```sh
sudo chown -R prometheus:prometheus /opt/prometheus
```

Create a new systemd service file called
`/etc/systemd/system/prometheus.service`.

```
[Unit]
Description=Prometheus Monitoring
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
WorkingDirectory=/opt/prometheus
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml
ReadWriteDirectories=/opt/prometheus

[Install]
WantedBy=multi-user.target
```

Reload systemd daemons.

```sh
sudo systemctl daemon-reload
```

Start the Prometheus service.

```sh
sudo systemctl start prometheus
```

Enable the Prometheus service so it starts when the system starts.

```sh
sudo systemctl enable prometheus
```

Verify it works by going to http://[your server's IP address/hostname]:9090.

If you're using a virtual machine, are working from the host machine, and you
have port 9090 forwarded, you can go to http://localhost:9090 on the host
machine.

## Add Authentication

Right now, anyone who visits the URL will see all monitoring information. This
could leak sensitive data. Prometheus has built-in basic authentication
available for configuration.

First, a password hash must be created. You can use Python for this. Debian
Linux comes preinstalled with Python 3.

Install `python3-bcrypt`.

```sh
sudo apt update
sudo apt install -y python3-bcrypt
```

Create a file called `gen-pass.py`.

```python
import getpass
import bcrypt

password = getpass.getpass("password: ")
hashed_password = bcrypt.hashpw(password.encode("utf-8"), bcrypt.gensalt())
print(hashed_password.decode())
```

Run the file with Python.

```sh
python3 gen-pass.py
```

You will be prompted for a password. Enter a strong and unique password that you
will use to log in to Prometheus's web user interface, then press **Enter**.

The password hash is output. Copy it.

Create a file called `/opt/prometheus/web.yml`.

```yaml
basic_auth_users:
    [username]: [copied password hash]
```

For the username, choose any username you like. For the password hash, paste the
password hash you copied earlier.

Edit `/etc/systemd/system/prometheus.service` to change the `ExecStart` line.

```
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml --web.config.file=/opt/prometheus/web.yml
```

Reload systemd daemons.

```sh
sudo systemctl daemon-reload
```

Restart the Prometheus service.

```sh
sudo systemctl restart prometheus
```

Now, when you go to http://localhost:9090, a username/password prompt will
appear. Enter the username and password you chose earlier to gain access.

## Adding an Exporter

Prometheus comes with some basic metrics to monitor by default, but you'll want
to use "exporters" which provide access to even more metrics. There are metrics
for your hardware and OS (the "Node exporter" `node_exporter`), MySQL/MariaDB
(the "MySQL Server Exporter" `mysqld_exporter`), and more.

Create a directory to store the exporters.

```sh
sudo mkdir /opt/prometheus/exporters
```

Go to https://prometheus.io/download/.

Under **Operating System**, choose **linux**.

Under **Architecture**, choose **amb64**.

In the **node_exporter** section, copy the listed URL. It will be labeled
something like **node_exporter-1.5.0.linux-amd64.tar.gz**.

In your terminal, download the file at the copied URL.

```sh
wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz
```

Unzip the file.

```sh
tar xvfz node_exporter-1.5.0.linux-amd64.tar.gz
```

Remove the compressed file.

```sh
rm node_exporter-1.5.0.linux-amd64.tar.gz
```

Move the exporter executable to the exporters directory.

```sh
sudo mv node_exporter-1.5.0.linux-amd64/node_exporter /opt/prometheus/exporters
```

Remove the remaining files.

```sh
rm -rf node_exporter-1.5.0.linux-amd64
```

The binary you just copied is still owned by your user account. Change the
ownership of it (and all other files in `/opt/prometheus` for good measure).

```sh
sudo chown -R prometheus:prometheus /opt/prometheus
```

Create a new systemd service file at
`/etc/systemd/system/prometheus_node_exporter.service`.

```
[Unit]
Description=Prometheus Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/opt/prometheus/exporters/node_exporter

[Install]
WantedBy=multi-user.target
```

Reload systemd daemons

```sh
sudo systemctl daemon-reload
```

Start the exporter

```sh
sudo systemctl start prometheus_node_exporter
```

Enable the exporter so it starts when the system starts.

```sh
sudo systemctl enable prometheus_node_exporter
```

Edit `/opt/prometheus/prometheus.yml`.

Indented under the `scrape_configs` section, add the following.

```
- job_name: node
  static_configs:
  - targets: ['localhost:9100']
```

Restart the Prometheus service.

```sh
sudo systemctl restart prometheus
```

Verify it works by going to
http://localhost:9090/graph?g0.expr=rate(node_disk_io_time_seconds_total[1m]).
That page will show you the rate of I/O operations of your system disks.
