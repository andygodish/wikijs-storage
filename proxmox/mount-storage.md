---
title: Using the CLI to Mount an NFS Drive
description: How to resolve an issue I was running into trying to mount an NFS to a proxmox node.
published: true
date: 2022-12-21T06:37:54.655Z
tags: storage, proxmox
editor: markdown
dateCreated: 2022-12-21T06:37:54.655Z
---

# Using the CLI to Mount an NFS Drive

On one of my older nodes was failing to attach an NFS mount through the UI, complaining of an error outlined in the following reference post from the Proxmox forum: 

- [Reference Post](https://forum.proxmox.com/threads/cifs-issue-error-with-cfs-lock-file-storage_cfg-working-now-but-shows-question-mark.45962/)

The fix was to follow the second command from the reference post. 

You can see these mounts at `/mnt/pve`, with each directory representing a storage object in the Datacenter. 

## Listing Available Volumes on NFS 

```
pvesm cifsscan 192.168.1.55 --username <username> --password <password>
```

This will show you the available volumes you can use in the subseqent command: 

```
pvesm add cifs <name-of-storage-object> --server 192.168.1.55 --share ProxmoxBackup --username <username> --password <password>
```
