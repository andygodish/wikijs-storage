---
title: Initial Configuration
description: Initial setup steps for configuration a new proxmox server.
published: true
date: 2022-08-12T03:20:41.883Z
tags: proxmox
editor: markdown
dateCreated: 2022-08-12T03:20:41.883Z
---

# Initial Configuration

Follow the iso boot from a usb device, I often wind up googling the first several configuration steps required to set up a Proxmox server the way I like it. 

This guide was writtent using pe v7.2

## Enable Updates

SSH into the server using the root user configured during installation. 

Edit the following files:

```
vim /etc/apt/source.list
---
## Add the two lines below (including the comment)
# not for production
deb http://download.proxmox.com/debian buster pve-no-subscription
```

The pve-enterprise.list contains a single line by default, comment it out.

```
vim /etc/apt/sources.list.d/pve-enterprise.list
```

Update & Upgrade

```
apt update 
apt upgrade
```

This begs the question, is updating via apt not available prior to these configuration changes?

## Increase Storage

Since two of my nodes are tiny NUCs and I don't have my NAS server configured, I want to utilize all 240 GB. By default, the harddisk is separated into two blocks. The second (hostname-lvm) uses most of the storage available, and is not assignable to nodes. 

1. Navigate to Datacenter --> Storage
		- Delete 'hostname'-lvm
    
2. SSH into the box and run the following commands:

```
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
```

3. Navigate to Datacenter --> Storage

Click `local` and then the `edit` button. In the `Content` dropdown, check **Disk image** and **Containers**. Click `Ok`.
    