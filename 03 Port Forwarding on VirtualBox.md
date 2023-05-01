# Linux Server

## Port Forwarding on VirtualBox

This guide is only necessary if your server is running on a virtual machine. Any
time you need to connect to your virtual machine remotely, you'll need to make
sure the appropriate port is forwarded.

The steps below focus on SSH and port 22, but you can use this guide for any
service and port.

Open **Oracle VM VirtualBox Manager** on the host machine.

Click to select the virtual machine from the list on the left.

Click the **Settings** button.

Click **Network** from the pane on the left.

Under the **Adapter 1** tab, click **Advanced**.

Click **Port Forwarding**.

Click the **Adds new port forwarding rule.** button. It's on the right side and
it looks like a green diamond with a green "plus" on top of it.

In the table on the left, double-click **Rule 1**. This will allow you to edit
the name of the rule. Type in the name of the service. For example, **SSH**.

Under **Host Port**, enter the port number you want the host machine to listen
on. This can be anything, but for simplicity, I recommend using the same as the
guest port. For SSH, use port **22**.

Note: If you have multiple virtual machines which you will be SSHing into, or
you already have an SSH server running on the host machine itself, you may want
to enter a different **Host Port**. In that case, choose any number you like
between 1024 and 49152. Just make sure it is not in use by any other service.

Under **Guest Port**, enter the port number that your virtual machine is
listening on. For SSH, use port **22**.

Click **OK**.

Click **OK** again.

### Other Ports

For this series of guides, we will be setting up services that listen on the following ports. You should set up port forwarding for each of them:

- HTTP, port 80
- HTTPS, port 443
- MariaDB, port 3306
- Node.js, port 3000
- Prometheus, port 9090
- SSH, port 22
