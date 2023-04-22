# Linux Server

## nftables Firewall Configuration

Debian Linux comes with nftables installed by default.

Edit the nftables configuration file located at `/etc/nftables.conf`

```
#!/usr/sbin/nft -f

flush ruleset

table inet filter {
        chain input {
                type filter hook input priority 0;

                # Allow connections on local machine
                iifname lo accept;

                # Allow established/related connections
                ct state established,related accept;

                # Be pingable
                icmp type echo-request accept;

                # Allow SSH and HTTP from anywhere
                tcp dport {ssh,http} accept;

                # Block everything else
                policy drop;
        }
        chain forward {
                type filter hook forward priority 0;
        }
        chain output {
                type filter hook output priority 0;
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
