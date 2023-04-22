# Linux Server

## Install the Operating System

### Prerequisites

- A physical machine is ready for fresh operating system installation or a new
  virtual machine has been created

### Download

Go to the [Installing Debian via the Internet
page](https://www.debian.org/distrib/netinst).

Under the section labeled **Small CDs or USB sticks**, click the architecture
your server uses. If you're not sure, it's probably **amd64**.

The ISO image will start downloading. Make a note of where this ISO image is
stored on your computer.

### Create Installation Media

If you're setting up the server on a virtual machine, you can skip this section.
Otherwise, if you're setting up a physical machine, you'll need to follow the
steps in this section to create a bootable flash drive with the ISO loaded.

Go to [rufus.ie](https://rufus.ie).

Under the **Download** section, click **Rufus 3.22** (the version number might
be different).

Once the download completes, run the installer and follow its instructions to
install Rufus.

Plug a USB flash drive into your machine.

Warning: The contents of this flash drive will be erased in the next few steps.
Make a copy if you don't want to lose anything.

Click **Start** and look for **Rufus**. Click it to launch Rufus.

Under **Device**, select the flash drive you want to use.

Under **Boot selection**, click **SELECT**.

Navigate to where the ISO image you downloaded earlier is stored. Click to
highlight it, then click **Open**.

Click **START**.

Once Rufus is finished, remove the flash drive from your machine and plug it
into the server machine you're setting up. Make sure the machine is off.

### Attaching the ISO

If you're setting up the server on a physical machine, you can skip this
section. Otherwise, if you're setting up a virtual machine, you'll need to
follow the steps in this section to attach the ISO image you downloaded earlier
as an optical disk.

Open **Oracle VM VirtualBox Manager**.

Look for the list of virtual machines on the left. Click to highlight the
virtual machine to which you want to install the operating system.

Click the **Settings** button.

Click **Storage** from the pane on the left.

Under **Storage Devices**, click to highlight **Controller: SATA**

Click the **Adds optical drive.** button. It is next to "Controller: SATA" and
looks like a blue disc with a green "plus" over top of it.

The **Optical Disk Selector** window appears. Click the **Add** button.

Navigate to where the ISO image you downloaded earlier is stored. Click to
highlight it, then click **Open**.

Click **Choose**.

Click **OK**.

### Install the Operating System

If you're setting up a physical machine, press the power button to boot it up.
You may need to press a specific key on the keyboard to access the boot menu.
Watch your screen for instructions. If you miss it, you can restart the machine
and try again. The goal is to get to the boot menu and boot from the inserted
USB drive that has the Debian installation ISO loaded.

Otherwise, if you're setting up a virtual machine, click the **Start** button in
**Oracle VM VirtualBox Manager**. Shortly, a window will appear containing your
virtual machine.

Make sure **Graphical install** is selected and press **Enter**.

Choose your language and click **Continue**.

Choose your location and click **Continue**.

Choose the keymap your keyboard uses and click **Continue**.

Under **Hostname**, enter a descriptive hostname. Example: "myserver".
Making the virtual machine's name in VirtualBox and the hostname here match is a
good idea.

Click **Continue**.

Under **Domain name**, enter the domain name for this machine. Make sure it
matches other machines on your network. If you don't have any other machines on
the network configured for a specific domain, you can choose any domain name
here. Example: "localdomain".

Click **Continue**.

Under **Root password**, choose and enter a strong, unique password for the root
user.

Under **Re-enter password to verify**, enter the same password again.

Do not forget this password. You may want to record it in some password
management software.

Click **Continue**.

Under **Full name for the new user**, enter your name. First and last is good
enough. It's just something to identify you on the system.

Click **Continue**.

Under **Username for your account**, enter the username you want to log in with.
Your first name, all-lowercase is a good option. Another option is your first
initial and your last name, all-lowercase.

Click **Continue**.

Under **Choose a password for the new user**, enter a strong, unique password
that you will log in with.

Under **Re-enter password to verify**, enter the same password again.

Do not forget this password. You may want to record it in some password
management software.

Click **Continue**.

Under **Select your time zone**, click to highlight the timezone in use where
you are located.

Click **Continue**.

Under **Partitioning method**, click to highlight **Guided - use entire disk and
set up LVM**.

Click **Continue**.

Under **Select disk to partition**, make sure the correct hard disk is selected.
There may only be one option. The size should be near what you entered for the
**Disk Size** when creating the virtual machine.

Click **Continue**.

Under **Partitioning scheme**, leave **All files in one partition (recommended
for new users)** selected.

Click **Continue**.

Under **Write the changes to disks and configure LVM?**, choose **Yes**.

Click **Continue**.

Under **Amount of volume group to use for guided partitioning**, leave the
default entered.

Note: The default value should be the maximum. You can also enter "max" to
ensure the volume group is the maximum size.

Click **Continue**.

If you see a message about "UEFI" and the installer asks you if you want to
**Force UEFI installation?**, choose **Yes** and click **Continue**.

Under **Write the changes to disks?**, choose **Yes**.

Click **Continue**.

Under **Scan extra installation media?**, leave **No** selected.

Click **Continue**.

Under **Debian archive mirror country**, choose your country or the closest
country to you.

Click **Continue**.

Under **Debian archive mirror**, leave the default selected.

Click **Continue**.

Leave the textbox for **HTTP proxy information** blank and click **Continue**.

Under **Participate in the package usage survey?**, leave **No** selected and
click **Continue**.

Under **Choose softare to install**, uncheck *everything* and then check **SSH
server** and **standard system utilities**.

Click **Continue**.

After a few moments, a message will appear stating that the installation is
complete. Click **Continue**.

The machine will reboot and the login screen will appear.

### Set up `sudo`

Logging in and doing things as the `root` user is not recommended. Instead, you
should log in and do things with you user account, and only do things as `root`
when necessary using `sudo`. We have to install and configure `sudo` first.

Log in as `root` with the password you chose during installation.

Install `sudo`.

```sh
apt install sudo
```

When asked to continue, press **Enter**.

Add your user account to the `sudo` group.

```sh
usermod -aG sudo [your username]
```

Make sure to replace `[your username]` with the username you chose during
installation. All members of the `sudo` group can execute commands as `root`
when necessary using `sudo`.

You are now ready to use your new server.

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

Under **Snapshot Name**, enter "Operating System Installed" and click **Ok**.

Click the **Start** button again. The virtual machine will boot back up.
