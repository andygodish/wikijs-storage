---
title: Clustering
description: How-to cluster Proxmox Nodes
published: true
date: 2022-08-12T04:18:10.248Z
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