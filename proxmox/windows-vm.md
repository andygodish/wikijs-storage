---
title: Setting Up a Windows VM
description: How-to set up a Windows VM on proxmox.
published: true
date: 2022-09-23T23:07:43.828Z
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

- Network Model: VirtIO (paravirtualized)

Don't start after creating - you need to add another disk for the virtio driver. 

## Add another CD-ROM Drive

Select you VM --> `Hardward`. Then click Add --> `CD/DVD Drive`. Select the appropriate storage containing your virtio drivers. 

Sed the Bus/Device: IDE, 3

You can now start your VM for the first time.

## Windows Installer

Go through the installation process once started up. Nothing much to note from this process. 

Once complete, check the `Device Manager` for `other devices`. These are devices unknown to the machine because the required driver is not detected on the machine. Right click each one and select `Update Driver`. Select the external disk (D drive in my case) and go through the update process. 

## Install Qemu Agent

Navigate to you D drive (Virtio Drivers for Proxmox) and find the guest-agent folder. Install the 64 bit version. 

