TODO: Polish this document.

# Linux Server

## Monitoring with Prometheus

https://prometheus.io/download/

OS: linux

Copy file name link

wget https://github.com/prometheus/prometheus/releases/download/v2.37.6/prometheus-2.37.6.linux-amd64.tar.gz

(Use actual file name)

tar xvfz prometheus-2.37.6.linux-amd64.tar.gz

rm prometheus-2.37.6.linux-amd64.tar.gz

sudo mv prometheus-2.37.6.linux-amd64 /opt/prometheus

sudo useradd --no-create-home --shell /usr/sbin/nologin prometheus

sudo chown -R prometheus:prometheus /opt/prometheus

vim /etc/systemd/system/prometheus.service

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

sudo systemctl daemon-reload
sudo systemctl start prometheus
sudo systemctl enable prometheus

### Firewall Configuration

If you have a firewall running, you'll need to allow connections on Prometheus's
default port 9090.

Edit `/etc/nftables.conf`.

```
table inet filter {
    chain input {
        # Allow Prometheus
        tcp dport 9090 accept;
    }
}
```

Restart the nftables service.

```sh
sudo systemctl restart nftables
```

Verify it works by going to localhost:9090

### Add Authentication

Create `gen-pass.py`

```python
import getpass
import bcrypt

password = getpass.getpass("password: ")
hashed_password = bcrypt.hashpw(password.encode("utf-8"), bcrypt.gensalt())
print(hashed_password.decode())
```

python3 gen-pass.py

Enter any password.

The password hash is output. Copy it.

Create `/opt/prometheus/web.yml`

```yaml
basic_auth_users:
    [username]: [copied password hash]
```

Edit `/etc/systemd/system/prometheus.service`

ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml --web.config.file=/opt/prometheus/web.yml

sudo systemctl daemon-reload
sudo systemctl restart prometheus

Visit http://localhost:9090. A username/password prompt will appear.

### Exporter

sudo mkdir /opt/prometheus/exporters

https://prometheus.io/download/

OS: Linux

Scroll to node_exporter

Copy URL

wget https://github.com/prometheus/node_exporter/releases/download/v1.5.0/node_exporter-1.5.0.linux-amd64.tar.gz

(Use actual URL)

tar xvfz node_exporter-1.5.0.linux-amd64.tar.gz

rm node_exporter-1.5.0.linux-amd64.tar.gz

sudo mv node_exporter-1.5.0.linux-amd64/node_exporter /opt/prometheus/exporters

rm -rf node_exporter-1.5.0.linux-amd64

sudo chown -R prometheus:prometheus /opt/prometheus

sudo vim /etc/systemd/system/prometheus_node_exporter.service

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

sudo systemctl daemon-reload
sudo systemctl start prometheus_node_exporter
sudo systemctl enable prometheus_node_exporter

Verify it works by going to localhost:9100/metrics

#### Update Prometheus Config

vim /opt/prometheus/prometheus.yml

Indented under the `scrape_configs` section:

```
- job_name: node
  static_configs:
  - targets: ['localhost:9100']
```

sudo systemctl restart prometheus

Verify it works by going to http://localhost:9090/graph?g0.expr=rate(node_disk_io_time_seconds_total[1m])
