---
label: Database Backups
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -17
---

# Database Backups

## Prerequisites

- MariaDB installed
- One or more databases created
- MariaDB has a `root` user which authenticates with the local filesystem
  account

## Install the Backup Utility

```sh
sudo apt update
sudo apt install -y mariadb-backup
```

## Create a Backup Script

Create a backup script at `/usr/local/bin/backup_databases.sh`.

```sh
#!/bin/bash

# Enable errexit so the script stops as soon as it encounters an error
set -e

# Create directory for backups
mkdir -p /var/mariadb/backup

# Store the backup filename
FILENAME=/var/mariadb/backup/backup_$(date -u +%Y%m%dT%H%M%SZ).gz

# Use mariabackup to create a compressed backup of the database
mariabackup --user=root --backup --stream=xbstream | gzip > "$FILENAME"

# Was the backup file created?
if [ -f "$FILENAME" ]; then
        # Is the file too small to contain data?
        if [ $(stat -c "%s" "$FILENAME") -lt 100000 ]; then
                # Remove the junk file. Output error. Exit.
                rm "$FILENAME"
                echo "Backup failed: The backup file contained no data" >&2
                exit 1
        fi
else
        # Output error. Exit.
        echo "Backup failed: No backup file was created" >&2
        exit 1
fi

# Count the number of backup files
NUM_BACKUPS=$(find /var/mariadb/backup -type f -name "backup_*\.gz" | wc -l)

# If there are multiple backup files...
if [ $NUM_BACKUPS -gt 1 ]; then
        # Remove backup files that are 14 days old or older
        find /var/mariadb/backup -type f -name "backup_*\.gz" -mtime +14 -exec rm {} \;
fi
```

Make the script executable.

```sh
sudo chmod +x /usr/local/bin/backup_databases.sh
```
## Configure Automatic Backups with systemd

Create `/etc/systemd/system/backup_databases.service`.

```
[Unit]
Description=Backup Databases
After=mariadb.service
Requires=mariadb.service

[Service]
Type=simple
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

Enable the timer so it starts when the system starts.

```sh
sudo systemctl enable backup_databases.timer
```

Start the timer now.

```sh
sudo systemctl start backup_databases.timer
```

At this point, all MariaDB databases will be backed up to `/var/mariadb/backup`
every day at midnight.

## Manually Backing Up Databases

You can back up databases at any time.

```sh
sudo mariabackup --user=root --backup --stream=xbstream | sudo gzip > /var/mariadb/backup/backup_$(date -u +%Y%m%dT%H%M%SZ).gz
```

Or you could run the backup script.

```sh
sudo /usr/local/bin/backup_databases.sh
```

!!!warning
Running the backup script will run the entire script, including the command
which removes backups 14 days or older.
!!!
