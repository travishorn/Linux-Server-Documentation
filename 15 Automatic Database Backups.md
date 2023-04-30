# Linux Server

## Automatic Database Backups

### Prerequisites

- MariaDB installed
- One or more databases created
- MariaDB has a `root` user which authenticates with the local filesystem
  account

### Install the Backup Utility

Make sure your system is up to date.

```sh
sudo apt update
sudo apt upgrade
```

Install mariadb-backup

```sh
sudo apt install mariadb-backup
```

### Create a Backup Script

Create a backup script at `/usr/local/bin/backup_databases.sh`.

```
#!/bin/bash

# Enable errexit so the script stops as soon as it encounters an error
set -e

# Create directory for backups
mkdir -p /var/mariadb/backup

# Use mariadb-backup to create a compressed backup of the database
mariadb-backup --user=root --backup --stream=xbstream | gzip > /var/mariadb/backup/backup_$(date -u +%Y-%m-%dT%H:%M:%SZ).gz

# Remove files 14 days old or older
find /var/mariadb/backup -type f -mtime +14 -exec rm {} \;
```

Make the script executable.

```sh
sudo chmod +x /usr/local/bin/backup_databases.sh
```
### Configure systemd Service and Timer

Create `/etc/systemd/system/backup_databases.service`.

```
[Unit]
Description=Backup Databases

[Service]
Type=oneshot
ExecStart=/usr/local/bin/backup_databases.sh

[Install]
WantedBy=multi-user.target
```

Create `/etc/systemd/system/backup_databases.timer`.

```
[Unit]
Description=Run Database Backup Script Daily

[Timer]
OnCalendar=daily
AccuracySec=1s
Persistent=true

[Install]
WantedBy=timers.target
```

Reload the system daemon to pick up the new service and timer.

```sh
sudo systemctl daemon-reload
```

Start the timer.

```sh
sudo systemctl start backup_databases.timer
```

At this point, all MariaDB databases will be backed up to `/var/mariadb/backup`
every day at midnight.

### Manually Backing Up Databases

You can back up databases at any time.

```sh
sudo mariadb-backup --user=root --backup --stream=xbstream | sudo gzip > /var/mariadb/backup/backup_$(date -u +%Y-%m-%dT%H:%M:%SZ).gz
```

Or you could run the backup script.

```sh
sudo /usr/local/bin/backup_databases.sh
```

Note: This will run the entire script, including the command which removes
backups 14 days or older.
