# Linux Server

## Set Up a MariaDB Database

### Install MariaDB

```sh
sudo apt install -y mariadb-server
```

### Create a Database and a User

Log in as the root system user.

```sh
sudo mysql
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

### Forward the Port on VirtualBox

If you are using VirtualBox and you will be connecting to MariaDB from the host
or a remote machine, you will need to forward port 3306.

Open **Oracle VM VirtualBox Manager**.

Click to highlight the server virtual machine.

Click on **Settings**.

Click **Network** in the pane on the left.

Under the **Adapter 1** tab, click **Advanced**.

Click **Port Forwarding**.

Click the **Adds new port forwarding rule.** button on the right. It looks like
a green diamond with a green "plus" sign on top of it.

Under name, enter **MariaDB**.

Under **Host Port**, enter **3306**.

Under **Guest Port**, enter **3306**.

Note that you can change the **Host Port** to another number if you are hosting
other databases on port 3306 on other virtual machines or port 3306 is otherwise
already in use on the host machine.

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
