---
label: Storing Data with MariaDB
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -16
---

# Storing Data with MariaDB

## Prerequisites

- A firewall rule to accept traffic on TCP port 3306 has been added
- If using a virtual machine, port 3306 is forwarded

## Install MariaDB

```sh
sudo apt update
sudo apt install -y mariadb-server
```

## Create a Database and a User

Log in as the root system user.

```sh
sudo mariadb
```

Create a database for your web application.

```sql
CREATE DATABASE mydatabase;
```

Create a user to represent your web application.

```sql
CREATE USER '[username]'@'%' IDENTIFIED BY '[password]';
```

Replace `[username]` and `[password]` with the actual credentials you want your
web applications to use when they authenticate to the database.

!!!warning
The `%` means that the user can connect from any IP address. Replace it with
`localhost` to only allow local connections or replace it with an IP address to
only allow connections from that IP address.
!!!

Grant database privileges to this user.

```sql
GRANT ALL PRIVILEGES ON mydatabase.* TO '[username]'@'%';
```

!!!warning
The same warning as above applies to this `%`.
!!!

Apply the privilege changes.

```sql
FLUSH PRIVILEGES;
```

Exit the MariaDB REPL.

```sql
EXIT;
```

## Allow Remote Connections

Edit `/etc/mysql/my.cnf`.

Add the following lines at the end of the file.

```
[mysqld]
bind-address = 0.0.0.0
```

Save and close the file.

Restart the database service.

```sh
sudo systemctl restart mariadb
```
