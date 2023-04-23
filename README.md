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

- you can connect to remotely,
- can host bare source code repositories,
- runs production web applications with continuous integration
- serves content through a reverse-proxy over HTTP and HTTPS
- is secured with a firewall
- hosts a database
- integrates database connectivity with a web app
- provides web-based system monitoring

The end product is designed to be a baseline that you can extend with more
functionality or harden for additional security depending on your needs.

## Table of Contents

1. [Create a Virtual Machine](./01%20Create%20a%20Virtual%20Machine.md)
2. [Install the Operating System](./02%20Install%20the%20Operating%20System.md)
3. [Connect via SSH](./03%20Connect%20via%20SSH.md)
4. [Set up a git Server](./04%20Set%20up%20git%20Server.md)
5. [Create a New git Repository](./05%20Create%20New%20git%20Repository.md)
6. [Pull and Push to a git
   Repository](./06%20Pull%20and%20Push%20to%20git%20Repository.md)
7. [Install Node.js](./07%20Install%20Node.js.md)
8. [Serve a Next.js App](./08%20Serve%20Next.js%20App.md)
9. [Manage Node.js Processes with pm2](./09%20Manage%20Node.js%20Processes.md)
10. [Continuous Integration](./10%20Continuous%20Integration.md)
11. [Reverse Proxy Using nginx](./11%20Reverse%20Proxy%20Using%20nginx.md)
12. [nftables Firewall
    Configuration](./12%20nftables%20Firewall%20Configuration.md)
13. [HTTPS/SSL on nginx](./13%20HTTPS%20SSL%20on%20nginx.md)
14. [Set Up a MariaDB Database](./14%20Set%20Up%20MariaDB%20Database.md)
15. [Connect to MariaDB from a Next.js
    App](./15%20Connect%20to%20DB%20from%20Next.js.md)
16. [Monitoring with Prometheus](./16%20Monitoring%20with%20Prometheus.md)

## To Do

- SSH authentication with public/private keys
  - Generating keys
  - Adding authorized keys to system user accounts
  - Setting up client configuration with identity files
- git remotes via SSH keys
  - Authorizing multiple devs with individual keys
  - Eliminate need to share passwords

## License

Copyright 2023 Travis Horn, licensed under [CC BY
4.0](http://creativecommons.org/licenses/by/4.0/)
