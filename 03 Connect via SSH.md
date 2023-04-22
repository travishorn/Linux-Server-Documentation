# Linux Server

## Connect via SSH

### Prerequisites

- Linux is installed and a user(s) has been created
- SSH is installed on your remote/host machine

### Configure the Virtual Machine

This section is only necessary if your server is running on a virtual machine.

If it isn't already, open **Oracle VM VirtualBox Manager** on the host machine.

Click to select the virtual machine from the list on the left.

Click the **Settings** button.

Click **Network** from the pane on the left.

Under the **Adapter 1** tab, click **Advanced**.

Click **Port Forwarding**.

Click the **Adds new port forwarding rule.** button. It's on the right side and
it looks like a green diamond with a green "plus" on top of it.

In the table on the left, double-click **Rule 1**. This will allow you to edit
the name of the rule. Name it "SSH".

Under **Host Port**, enter **22**.

Note: If you have multiple virtual machines or you have an SSH server running on
the host machine, you may want to enter a different **Host Port**. In that case,
choose any number you like between 1024 and 49152. Just make sure it is not in
use by any other service.

Under **Guest Port**, enter **22**.

Click **OK**.

Click **OK** again.

## Connect

On your remote machine (note this could even be the host machine), open up a
**Terminal**.

Connect to the server via SSH

```sh
ssh [your username]@[your server's IP address/hostname]
```

Be sure to replace `[your username]` with your actual username on the server and
replace `[your server's IP address/hostname]` with your server's actual IP
address or hostname.

If you're using a virtual machine, you can use `localhost` as your server's
hostname.

If you're using a virtual machine and you set a different **Host Port** in the
section above, you must add ` -p [port number]` to the end of the command.

If asked if you want to continue connecting, type `yes` and press **Enter**

You will be prompted for your user's password on the server. Enter it and press
**Enter**.

You are now logged in to your server remotely via SSH.
