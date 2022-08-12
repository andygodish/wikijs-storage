---
title: Clustering
description: How-to cluster Proxmox Nodes
published: true
date: 2022-08-12T04:34:52.945Z
tags: proxmox
editor: markdown
dateCreated: 2022-08-12T04:18:10.248Z
---

# Clustering

## Removing Existing Cluster Configuration

[Reference Post](https://forum.proxmox.com/threads/proxmox-ve-6-removing-cluster-configuration.56259/post-259203
)

Follow these steps after SSHing into your Proxmox server:

```
systemctl stop pve-cluster corosync
pmxcfs -l
rm /etc/corosync/*
rm /etc/pve/corosync.conf
killall pmxcfs
systemctl start pve-cluster
```

This was prompted by an issue with my `/etc/hosts` file on my original pve server (the one where the cluster was created from). The old `10.` IP was being used to connect to the other node which utilized my new `192.168.1.0/24` subnet.

