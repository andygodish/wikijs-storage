---
title: Setting Up a Windows VM
description: How-to set up a Windows VM on proxmox.
published: true
date: 2022-09-12T20:17:53.048Z
tags: proxmox, windows
editor: markdown
dateCreated: 2022-09-12T20:17:53.048Z
---

# Windows VM


## ISO Downloads

- Windows Server 2019
- Windows 10
- Windows Drivers for Proxmox

It'll be the same process for any type of windows product, in this example, I'm installing server 2019.

## Creating the VM

- Make sure the type is set to Microsoft Windows along with its correct version.

- Qemu Agent checked

- SCSI Controller --> VirtIO SCSI 

- Bus/Device --> SCSI

- 64GB Disk

- Discard checked

- Write Back Cache 

- 2-4 Cores

- 4GB of memory

Don't start after creating - you need to add another disk for the virtio driver. 

## Add another CD-ROM Drive

Select you VM --> `Hardward`. Then click Add --> `CD/DVD Drive`. 







