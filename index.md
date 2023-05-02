---
label: Linux Server Documentation
author:
  name: Travis Horn
  email: travis@travishorn.com
icon: home
order: 0
---

# Linux Server Documentation

A small collection of simple instructions for setting up and configuring a Linux
server from scratch.

This guide is intended for novice system administrators or even advanced ones
that are not familiar with Linux, nginx, or some of the other covered topics.

Includes OS, SSH, git, Node.js, nginx, MariaDB, and more. Topics are split into
ordered pages for easy viewing. While each section does build upon the previous
ones, the configuration is basic and common enough that you may be able to jump
to any section and follow the instructions from there.

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
