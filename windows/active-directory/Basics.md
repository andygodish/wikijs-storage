---
title: Active Directory
description: Basics of Active Directory pertaining to its use in my homelab.
published: true
date: 2023-10-13T03:25:29.177Z
tags: windows, sysadmin, active directory
editor: markdown
dateCreated: 2023-10-12T23:26:07.844Z
---

# Active Directory

I'm running a Windows Server 2022 configured as a Domain Controller with Active Aiirectory in my homelab.

[This wiki post](https://github.com/andygodish/wikijs-storage/blob/main/proxmox/windows-vm.md) outlines the installation setup on a Proxmox node using an evaluation license from Microsoft (180 days for Windows Server).

My local DNS maps an A Record to the IP of my AD server at `ad.lan.andyg.io`

## Connection to AD Server

```
ping ad.lan.andyg.io
```

### LDAP communication typically uses two ports:

- 389: Standard port for LDAP. It's used for requesting and receiving information from the LDAP directory.

- 636: Used for Secure LDAP (LDAPS) which encrypts the communication using SSL/TLS.

I like to use `netcat` when testing open ports:

```
nc -zv ad.lan.andyg.io 389
```

`telnet` works as well:

```
telnet ad.lan.andyg.io 389
```

>SSL/TLS: If you're using LDAPS (port 636), ensure that your Wiki.js instance trusts the CA that issued the LDAP server's SSL certificate.

### LDAP URL

The LDAP URL: `ldap://ad.lan.andyg.io` 

This is the scheme (*ldap:// prefix*) that tells the client to use the LDAP protocol.