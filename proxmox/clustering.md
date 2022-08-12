---
title: Clustering
description: How-to cluster Proxmox Nodes
published: true
date: 2022-08-12T04:41:08.827Z
tags: proxmox
editor: markdown
dateCreated: 2022-08-12T04:18:10.248Z
---

# Clustering

## Create Cluster 

Pick a node, navigate to datacenter --> cluster. Click create cluster and verify the correct information. Pretty painless process.

## Join Cluster

On your OG node, click datacenter --> cluster --> join information. Copy it.

On the node you wish to join to the cluster, paste this info in and verify the information is correct. 

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

## TODO

*I was getting an error when trying to create a cluster from a node that had running VMs present. Not sure if this is possible. Do some research.*

