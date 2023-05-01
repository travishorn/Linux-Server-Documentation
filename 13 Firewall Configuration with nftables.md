# Linux Server

## Firewall Configuration with nftables

Debian Linux comes with nftables installed by default.

Edit the nftables configuration file located at `/etc/nftables.conf`

```
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
        chain input {
                type filter hook input priority 0;

                # Allow loopback (local connections)
                iifname lo accept

                # Allow established/related
                ct state established,related accept

                # Allow incoming pings
                ip protocol icmp limit rate 1/second accept

                # Allow SSH and HTTP
                tcp dport {ssh,http} accept

                # Drop everything else
                drop
        }
        chain forward {
                type filter hook forward priority 0;

                # Disallow forwarding
                drop
        }
        chain output {
                type filter hook output priority 0;

                # Allow all outgoing traffic
                accept
        }
}
```

Enable the nftables service so it starts when the machine starts.

```sh
sudo systemctl enable nftables
```

Start the nftables service now

```sh
sudo systemctl start nftables
```

If the service is already running and you just want to apply changes you
recently made to the configuration, just restart the service.

```sh
sudo systemctl restart nftables
```

### Add a New Firewall Rule

Any time you want to allow traffic for a new service on a specific port, you
must add a new firewall rule.

Edit the nftables configuration file located at `/etc/nftables.conf`

Find the line that looks like this:

```
tcp dport {ssh,http} accept
```

Add the new port into the comma-separated list inside curly braces. For example,
if you want to add a rule that allows port 3306, the line will look like this:

```
tcp dport {ssh,http,3306} accept
```

Note that some ports have aliases (like `ssh` and `http`) that nftables
recognizes.

Restart nftables to apply the new rules.

```sh
sudo systemctl restart nftables
```

### More Rules

For this series of guides, we will be setting up services that listen on the
following ports. You should set up firewall rules for all of them:

- SSH, port `22`, alias `ssh`
- HTTP, port `80`, alias `http`
- HTTPS, port `443`, alias `https`
- MariaDB, port `3306`, alias `mysql`
- Prometheus, port `9090`
