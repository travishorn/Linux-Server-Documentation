---
label: Create a Virtual Machine with VirtualBox
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -1
---

# Create a Virtual Machine with VirtualBox

If you don't already have it,
[download](https://www.virtualbox.org/wiki/Downloads) and install Oracle VM
VirtualBox Manager.

Open Oracle **VM VirtualBox Manager**.

Click the **New** button.

Give the virtual machine a name. Example: "myserver".

Under **Type**, choose **Linux**.

Under **Version**, choose the Linux version you will be installing. Example:
"Debian 11 Bullseye (64-bit)".

Click **Next**.

Under **Base Memory**, choose half of the available memory.

Under **Processors**, choose half of the available processors.

Check the box for **Enable EFI (special OSes only)**.

Click **Next**.

Leave **Create a Virtual Hard Disk Now** selected and enter a disk size. It
should probably be at least 20 GB. Choose a size that will accomodate any
software, application files, and database data you will need.

!!!
VirtualBox will not use all of the specified space immediately. It will only use
what it needs, up to a maximum of whatever you specify.
!!!

Click **Next**.

Click **Finish**.

Click the **Settings** button.

Click **System** from the pane on the left.

Under **Boot Order**, uncheck **Floppy** and then move it below **Hard Disk**.

Under **Chipset**, choose **ICH9**.

Click **Storage** from the pane on the left.

Under **Storage Devices**, click to highlight **Controller: IDE**.

Click the **Remove Controller** button. It is near the bottom of the window and
looks like a green diamond with a red "X" over top of it.

Click **Audio** from the pane on the left.

Uncheck **Enable Audio**.

Click **OK**.

At this point, you can continue by installing the operating system.
