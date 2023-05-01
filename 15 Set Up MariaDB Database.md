# Linux Server

## Set Up a MariaDB Database

### Prerequisites

- If using a virtual machine, port 3306 is forwarded

### Install MariaDB

```sh
sudo apt update
sudo apt install -y mariadb-server
```

### Create a Database and a User

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

Warning: The `%` means that the user can connect from any IP address. Replace it
with `localhost` to only allow local connections or replace it with an IP
address to only allow connections from that IP address.

Grant database privileges to this user.

```sql
GRANT ALL PRIVILEGES ON mydatabase.* TO '[username]'@'%';
```

Note: The same warning as above applies to this `%`.

Apply the privilege changes.

```sql
FLUSH PRIVILEGES;
```

Exit the MariaDB REPL.

```sql
EXIT;
```

### Allow Remote Connections

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

### Firewall Configuration

If you have a firewall running, you'll need to allow connections on MariaDB's
default port 3306.

Edit `/etc/nftables.conf`.

```
table inet filter {
    chain input {
        # Allow MariaDB
        tcp dport 3306 accept
    }
}
```

Restart the nftables service.

```sh
sudo systemctl restart nftables
```

### Take Another Snapshot

If you are setting up the server on a virtual machine, follow these steps to
take another snapshot.

Shut down the machine

```sh
shutdown now
```

In VirtualBox, make sure the machine is selected and then click **Machine** >
**Tools** > **Snapshots** from the menu at the top.

Click the **Take** button.

Under **Snapshot Name**, enter "MariaDB Installed" and click **Ok**.

Click the **Start** button again. The virtual machine will boot back up.
