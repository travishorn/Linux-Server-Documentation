# Linux Server

## Install Node.js

Update and upgrade existing packages

```sh
sudo apt update
sudo apt upgrade
```

In order to install Node.js, first install curl.

```sh
sudo apt install -y curl
```

Assume the `root` user

```sh
sudo -s
```

Install the latest LTS Node.js release

```sh
curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && apt-get install -y nodejs
```

Note that the above command(s) will install version 18.x. There may be a more
recent LTS release. You can find out at https://nodejs.org.

Exit the `root` user shell

```sh
exit
```

Make sure Node.js is installed correctly by getting its version number

```sh
node -v
```

It should display something like:

```sh
v18.16.0
```

The exact version number might vary.

### Create an App Directory

To stay organized, I recommend creating a directory to store any apps you might
deploy.

```sh
mkdir ~/apps
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

Under **Snapshot Name**, enter "Node.js Installed" and click **Ok**.

Click the **Start** button again. The virtual machine will boot back up.

You are now ready to run and serve Node.js web apps.
