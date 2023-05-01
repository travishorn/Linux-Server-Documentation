# Linux Server Documentation

A small collection of simple instructions for setting up and configuring a Linux
server from scratch.

This guide is intended for novice system administrators or even advanced ones
that are not familiar with Linux, nginx, or some of the other covered topics.

Includes OS, SSH, git, Node.js, nginx, MariaDB, and more. Sections are split
into ordered markdown files for easy viewing. While each section does build upon
the previous ones, the configuration is basic and common enough that you may be
able to jump to any section and follow the instructions from there.

If you follow this guide from start to finish, you will have a working Debian
Linux server that...

- you can connect to remotely
- can host bare source code repositories
- runs production web applications with continuous integration
- serves content through a reverse-proxy over HTTP and HTTPS
- is secured with a firewall
- hosts a database with automatic backups
- integrates database connectivity with a web app
- provides web-based system monitoring

The end product is designed to be a baseline that you can extend with more
functionality or harden for additional security depending on your needs.

## Table of Contents

1. [Create a Virtual Machine with VirtualBox](./01%20Create%20a%20Virtual%20Machine%20with%20VirtualBox.md)
2. [Install the Debian Linux Operating System](./02%20Install%20the%20Debian%20Linux%20Operating%20System.md)
3. [Port Forwarding in Virtualbox](./03%20Port%20Forwarding%20in%20Virtualbox.md)
4. [Access a Remote Shell with OpenSSH](./04%20Access%20a%20Remote%20Shell%20with%20OpenSSH.md)
5. [SSH Key Authentication](./05%20SSH%20Key%20Authentication.md)
6. [Set up a git Server](./06%20Set%20up%20a%20git%20Server.md)
7. [Create a New git Repository](./07%20Create%20a%20New%20git%20Repository.md)
8. [Pull from and Push to a git Repository](./08%20Pull%20from%20and%20Push%20to%20a%20git%20Repository.md)
9. [Install Node.js](./09%20Install%20Node.js.md)
10. [Serve a Next.js App](./10%20Serve%20a%20Next.js%20App.md)
11. [Manage Node.js Processes with PM2](./11%20Manage%20Node.js%20Processes%20with%20PM2.md)
12. [Continuous Integration](./12%20Continuous%20Integration.md)
13. [Reverse Proxy Using nginx](./13%20Reverse%20Proxy%20Using%20nginx.md)
14. [Firewall Configuration with nftables](./14%20Firewall%20Configuration%20with%20nftables.md)
15. [HTTPS SSL on nginx](./15%20HTTPS%20SSL%20on%20nginx.md)
16. [Storing Data with MariaDB](./16%20Storing%20Data%20with%20MariaDB.md)
17. [Database Backups](./17%20Database%20Backups.md)
18. [Database Backup Recovery](./18%20Database%20Backup%20Recovery.md)
19. [Connect to DB from a Next.js App](./19%20Connect%20to%20DB%20from%20a%20Next.js%20App.md)
20. [Monitoring with Prometheus](./20%20Monitoring%20with%20Prometheus.md)

## To Do

- SSH agent

## License

Copyright 2023 Travis Horn, licensed under [CC BY
4.0](http://creativecommons.org/licenses/by/4.0/)
