---
title: File System Disk Space Shkinkage
description: Not sure why, but following a power outage, the size of the root file system shrunk.
published: true
date: 2023-06-04T01:59:31.412Z
tags: linux, storage, proxmox
editor: markdown
dateCreated: 2023-06-04T01:46:53.561Z
---

# File System Disk Space Shkinkage

I was doing some electrical work that caused me to trip the circuit used by my server rack. Upon rebooting one of my Proxmox NUCs, I noticed that two VMs were failing to start with the following error message displayed in the Proxmox UI: 

```
  /etc/lvm/archive/.lvm_fred_25425_1689817242: write error failed: No space left on device
```

`df -h` revealed that my root file system was maxed out at 100% usage.

> for context, I utilize the entire internal harddrive in my Proxmox NUCs. You can see how that's configured in this [article](https://github.com/andygodish/wikijs-storage/blob/main/proxmox/initial-configuration.md).

## Logical Volume Came Back

I observed that the logical volume at `/dev/pve/data` had returned by running `lvdisplay`. In the linked article above, I use the following commands to remove that volume, like so:

```
lvremove /dev/pve/data
lvresize -l +100%FREE /dev/pve/root
resize2fs /dev/mapper/pve-root
```

However, with my root filesystem full, I was unable to execute these commands without erroring out. 

## Freeing Space on the Filesystem/Logical Volume

In order to free up space, I tried the following:

1. Clear Cache: You can clear the APT cache with sudo apt-get clean. This will remove all packages from the package cache. This can free up a significant amount of space if the cache has not been cleared in a while.

2. Remove Old Kernels: If you have old Linux kernels that are no longer needed, you can remove them with sudo apt autoremove. This can also free up a significant amount of space.

3. Delete Unnecessary Files: As mentioned before, you can try to delete unnecessary files or directories. Be careful with this step, as deleting the wrong files could cause issues with your server.

Options one and two did not work for me.

### Cleaning out large files

I found that the `/var/lib/vz/images` directory container almost all of the ~68GB data taking up space in my filesystem. 

This directory contained subdirectories for each VM/Container/Template ID, for example:

```
ls /var/lib/vz/images

03  107  108  110  901
```

> *Check the Size of Directories*: You can use the `du` command to check the size of directories. For example, `du -sh /*` will show the size of each directory in the root directory. The -s option means "summarize", so it will only show the total size for each directory, and the -h option means "human-readable", so it will show the sizes in a format that's easy to understand (like KB, MB, GB, etc.).

### Proxmox Disk Images

The directory /var/lib/vz/images is where Proxmox VE stores disk images for its virtual machines and containers. Each numbered subdirectory corresponds to a VM or container ID, and the .qcow2 files inside are the actual disk images for those VMs or containers.

The .qcow2 (QEMU Copy-On-Write) format is a common format for disk images in virtualization, used by QEMU and KVM. It's a flexible format that supports features like snapshots and can grow dynamically, only using as much physical disk space as actually needed by the data inside.

Here's what the files and directories mean:

*103, 107, 108, 110, 901*: These are the IDs of your VMs or containers. Each VM or container in Proxmox VE has a unique ID.

*vm-110-disk-0.qcow2*: This is the disk image for VM 110. The disk-0 part indicates that this is the first (or only) disk for that VM.

I chose to delete the 103 directory, which came in around 15GB in size, as its associated VM was no longer present on my Proxmox Node --- I verified this by observing that no machines were using the 103 id in the Proxmox UI. 

This freed me up to run commands against the /data logical volume. I removed it and resized the root volume to take up the available space. I restarted the node, and bam! My Node's root filesystem was now 250GB and my VMs started back up like normal. 