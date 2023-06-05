---
label: Automatic Updates
icon: file
author:
  name: Travis Horn
  email: travis@travishorn.com
order: -6
---

# Automatic Updates

## Prerequisites

- A running Debian Linux machine

## Install UnattendedUpgrades

Install `unattended-upgrades`.

```sh
sudo apt install -y unattended-upgrades
```

Activate it.

```sh
sudo dpkg-reconfigure -plow unattended-upgrades
```

By default, the system will check for and download upgrades twice a day:

- A random time in a 12 hour window starting at 6 AM
- A random time in a 12 hour window starting at 6 PM

It will then apply the upgrades once per day in a random time in a 60 minute
window starting at 6 AM.

## Modify the Schedule

You can override the default schedule when upgrades are applied.

```sh
sudo systemctl edit apt-daily-upgrade.timer
```

An editor will open. In the top section, enter the following.

```
[Timer]
OnCalendar=
OnCalendar=01:00
RandomizedDelaySec=0
```

The above example sets the upgrades to apply precisely at 1 AM every day.

The line `OnCalendar=` is necessary because, without it, any additional
`OnCalendar` lines simply **add** another time to the existing defaults. This
line clears the defaults first.

Once the overrides are in place, restart the timer.

```sh
sudo systemctl restart apt-daily-upgrade.timer
```

## Scheduled Reboots

Some upgrades require rebooting the system. One strategy is to set a specific
day of the month to check and perform a reboot if required.

Create the script to check and reboot if required at
`/usr/local/bin/reboot_if_required.sh`.

```sh
#!/bin/bash

if [ -f /var/run/reboot-required ]; then
  echo "Reboot required. Initiating reboot..."
  /sbin/shutdown -r now
else
  echo "No reboot required."
fi
```

Make the script executable.

```sh
sudo chmod +x reboot_if_required.sh
```

Create a systemd service for the script at
`/etc/systemd/system/unattended_reboot.service`.

```
[Unit]
Description=Reboot (if required)

[Service]
Type=oneshot
ExecStart=/usr/local/bin/reboot_if_required.sh

[Install]
WantedBy=default.target
```

Create a matching timer for the service at
`/etc/systemd/system/unattended_reboot.timer`.

```
[Unit]
Description=Reboot (if required) once per month on the 15th at 11 PM

[Timer]
OnCalendar=*-15 23:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

Enable the timer.

```sh
sudo systemctl enable unattended_reboot.timer
```

Start the timer.

```sh
sudo systemctl start unattended_reboot.timer
```

## Rebooting Immediately After Upgrades

You could take a more aggresive rebooting strategy instead. You can set the
system to automatically reboot after UnattendedUpgrades detects that one is
pending.

Edit `/etc/apt/apt.conf.d/50unattended-upgrades`. Uncomment this line:

```
//Unattended-Upgrade::Automatic-Reboot "false";
```

And change `false` to `true`.

```
Unattended-Upgrade::Automatic-Reboot "true";
```

## Manual Rebooting

If you don't use either of the two automatic options above, you can manually
reboot when it works for you. You can check if a reboot is pending by checking
for the presence of `/var/run/reboot-required`.

```sh
ls /var/run/reboot-required
```

If the file exists, consider rebooting at some point.

```sh
sudo shutdown -r now
```

Your server is now set up to apply automatic updates. At this point, you are
ready to set it up to host bare git repositories.
