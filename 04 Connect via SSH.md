# Linux Server

## Connect via SSH

### Prerequisites

- Linux is installed and a user(s) has been created
- SSH is installed on your remote/host machine
- If using a virtual machine, port 22 is forwarded

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

If you're using a virtual machine, you're connecting from the host machine, and
you have port 22 forwarded, you can use `localhost` as your server's hostname.

If you set up your SSH port to something other than `22`, you must add ` -p
[port number]` to the end of the command.

If asked if you want to continue connecting, type `yes` and press **Enter**

You will be prompted for your user's password on the server. Enter it and press
**Enter**.

You are now logged in to your server remotely via SSH.
