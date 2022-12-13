---
title: Clustering
description: How-to cluster Proxmox Nodes
published: true
date: 2022-12-13T23:02:45.923Z
tags: proxmox
editor: markdown
dateCreated: 2022-12-03T06:29:22.092Z
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

### Hanging Nodes 

After the previous steps are performed, I have observed that the Proxmox UI still shows the deleted node within the Datacenter. To remove this, remove the directory associated with the name of the node you wish to remove inside the `/etc/pve/nodes/` directory from the master node. 

## Updating the Expected Number of Nodes for Quorum

- [Forum Post](https://forum.proxmox.com/threads/can-not-login-to-proxmox-from-web-interface.32860/)

At one point, I had a two node pve cluster and decided to reimage one of the nodes with a fresh instal of Proxmox. I transferred all VMs to the second node and proceeded to wipe it. Upon trying to log into my second node, I was met with "login failed" messages. A `systemctl status pve-manager` revealed that the second node was "waiting for quorum..."

The following command kicked it back into gear: 

```
pvecm expected 1
```

This tells proxmox that it only needs a single healthy cluster node to start properly. 

## TODO

*I was getting an error when trying to create a cluster from a node that had running VMs present. Not sure if this is possible. Do some research.*

