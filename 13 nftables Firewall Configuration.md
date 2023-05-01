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
