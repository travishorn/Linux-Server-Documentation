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

## Rebooting

Some upgrades require rebooting the system. You can check if a reboot is pending
by checking for the presence of `/var/run/reboot-required`.

```sh
ls /var/run/reboot-required
```

If the file exists, consider rebooting at some point.

```sh
sudo shutdown -r now
```

### Automatic Rebooting

You can also set the system to automatically reboot after UnattendedUpgrades
detects that one is pending.

Edit `/etc/apt/apt.conf.d/50unattended-upgrades`. Uncomment this line:

```
//Unattended-Upgrade::Automatic-Reboot "false";
```

And change `false` to `true`.

```
Unattended-Upgrade::Automatic-Reboot "true";
```

Your server is now set up to apply automatic updates. At this point, you are
ready to set it up to host bare git repositories.
