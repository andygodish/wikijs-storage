---
title: Configure Centos to Receive an IPV4 from DHCP On Boot
description: I installed centos-stream-8 on my proxmox server and it did not initially receive an IPV4 on my LAN from my router's DHCP server.
published: true
date: 2022-07-19T03:45:42.919Z
tags: linux, networking, centos-stream, ipv4
editor: markdown
dateCreated: 2022-07-19T03:45:42.919Z
---

# Centos Stream 8 - IPV4 Onboot

Pretty simple configuration change, I ended up following [this guide](https://superuser.com/questions/1129554/centos-7-minimal-doesn-t-get-an-ip-address-from-dhcp-on-startup).

## Fix

- Check that `ONBOOT` is set to `yes` in `/etc/sysconfig/network-scripts/ifcfg-eno1677736`

Once complete, **reboot** your machine and verify an ip has been assigned to the correct interface, `ip addr`.

