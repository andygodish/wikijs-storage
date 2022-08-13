---
title: Stuck Cluster Nodes
description: How to remove cluster nodes that are stuck.
published: true
date: 2022-08-13T22:26:55.274Z
tags: proxmox, clustering
editor: markdown
dateCreated: 2022-08-13T22:26:55.274Z
---

# Remove Stuck Cluster Nodes

- [Proxmox Forum Comment](https://forum.proxmox.com/threads/deleted-cluster-node-still-shows-up-in-my-gui.11614/post-69162)

I ran into a scenario where one of my two proxmox nodes that comprised a cluster began having major networking issues following a power outage. Part of the troubleshooting involved opening a console session in the network-challenged node and manually removing all trace of a cluster from the command line. [See this wiki article](https://github.com/andygodish/wikijs-storage/blob/main/proxmox/clustering.md). 

Following a frustrating day of troubleshooting that seemed to have no effect on the state of the network, I gave up and decided to reinstall a fresh instance of proxmox. Of course, after leaving the machine idle for another day, the issue resolved itself, and I was able to get the VMs on that node back online. 

I recreated the cluster on the resotred node without issue. However, my second node still showed the other node in an unhealty state (appeared gray in the UI). To remove it, you have to delete its directory from, `/etc/pve/nodes/<nodename>` 

```
rm -rf /etc/pve/nodes/<nodename>
```

