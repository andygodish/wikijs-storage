---
title: Storage Configuration
description: How to set up storage configuration on my primary homelab server. 
published: true
date: 2022-12-30T22:00:05.692Z
tags: storage, homelab, sysadmin
editor: markdown
dateCreated: 2022-12-30T22:00:05.692Z
---

# Storage Configuration	

- [reference video](https://www.youtube.com/watch?v=odzs1i_JcmY&t=331s)

My primary homelab server requires that you configure virtual disks manually before they can be discovered by the underlying operating system. 

During the boot process, press `crtl+r` to enter the storage configuration menu. From there you can configure your storage devices into whatever RAID option applicable to your setup. In my case, I am running 4 SSDs, each in their own RAID0 configuration so I can use Proxmox to manage a single RAID 5 ZFS pool. 