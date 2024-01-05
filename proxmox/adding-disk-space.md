---
title: Adding Disk Space to a Proxmox VM
description: How to increase the disk space of a Proxmox VM. 
published: true
date: 2024-01-05T19:19:52.880Z
tags: storage, proxmox
editor: markdown
dateCreated: 2024-01-05T18:37:08.811Z
---

# Adding Disk Space to a Proxmox VM

I built a VM template using a Packer manifest that was configured to 8GB of disk space. I then tried to run a mysql docker container that immediately errored out with the following error found in the container logs: 

```
2024-01-05T17:05:12.626936Z 0 [ERROR] InnoDB: Write to file ./ibtmp1 failed at offset 11534336, 1048576 bytes should have been written, only 368640 were written. Operating system error number 28. Check that your OS and file system support files of this size. Check also that the disk is not full or a disk quota exceeded.
2024-01-05T17:05:12.626946Z 0 [ERROR] InnoDB: Error number 28 means 'No space left on device'
2024-01-05T17:05:12.626950Z 0 [Note] InnoDB: Some operating system error numbers are described at http://dev.mysql.com/doc/refman/5.7/en/operating-system-error-codes.html
2024-01-05T17:05:12.626956Z 0 [ERROR] InnoDB: Could not set the file size of './ibtmp1'. Probably out of disk space
```

## Adding Disk Space

I used the Proxmox GUI to add more disk space to the VM. Select your VM from the list > Hardware > Hard Disk > Disk Action > Resize

What you place in the `Size Increment (GiB)` field will add to the existing disk space. In my case, I entered 32 GiB and the result was a new `sda` disk of 40GB. However, my root partition remained 6.2 GB in size:

![lsblk-root-partition.png](/images/lsblk-root-partition.png)

## Enlarge the partition(s) in the virtual disk
- [Proxmox Docs](https://pve.proxmox.com/wiki/Resize_disks)

> From the docs: *Note that the partition you want to enlarge should be at the end of the disk.* 

In my case, the `lsblk` command shows that my root partition (sda3) is already located at the "end of the disk (sda)". If the partition you want to resize is at the end of the disk and there is unallocated space following it, you can usually resize it while the system is running (online resizing). 

### Viewing the Current Partition Table

![fdisk-list.png](/images/fdisk-list.png)

Now you can use `gparted` to resize partition 3 (sda3), run the following:

```
parted /dev/sda

resizepart 3 100%

quit
```
The partition should have been expanded to use the added space:
```
> lsblk
...
└─sda3                      8:3    0  38.2G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   6.2G  0 lvm  /
```

## Enlarge the filesystem(s) in the partitions on the virtual disk

The previous output shows that the `/` filesystem has not been expanded. 

First, enlarge the physical volume to occupy the whole available space in the partition:

```
pvresize /dev/sda3
```

where /dev/sda3 is the name of the physical volume (PV):

![pvs.png](/images/pvs.png)

---
![pvdisplay.png](/images/pvdisplay.png)


