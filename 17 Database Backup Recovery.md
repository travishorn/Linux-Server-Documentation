# Linux Server

## Database Backup Recovery

### Prerequisites

- The database is MariaDB
- `mariabackup` is installed
- You have a compressed backup file containing the state of the database to
  which you want to recover

### Take a Manual Backup

Before continuing with data recovery, I highly recommend creating a manual
backup, in case you need to revert back to the state before recovery started.
(See separate guide).

### Recover the Data

Create a directory for storing the recovered files temporarily. It can be named
anything you like. Here it is named `recovered`.

```sh
sudo mkdir /var/mariadb/backup/recovered
```

Unzip the compressed database backup in the directory.

```sh
sudo gunzip -c /var/mariadb/backup/backup_2023-04-30T03\:14\:58Z.gz | sudo mbstream -x --directory=/var/mariadb/backup/recovered
```

Prepare the recovery files. This is necessary because files created at backup
time are not point-in-time consistent and MariaDB will reject the recovery if
not properly prepared.

```sh
sudo mariabackup --prepare --target-dir=/var/mariadb/backup/recovered
```

Stop MariaDB.

```sh
sudo systemctl stop mariadb
```

Remove all existing data files.

```sh
sudo rm -rf /var/lib/mysql
```

Restore the backup files.

```sh
sudo mariabackup --copy-back --target-dir=/var/mariadb/backup/recovered
```

Change ownership of the newly recovered files so that the `mysql` user can
access them properly.

```sh
sudo chown -R mysql:mysql /var/lib/mysql
```

Start MariaDB.

```sh
sudo systemctl start mariadb
```

The database is now restored back to the state it was in when the backup was
taken.

Remove the recovery files as they are no longer needed.

```sh
sudo rm -rf /var/mariadb/backup/recovered
```
