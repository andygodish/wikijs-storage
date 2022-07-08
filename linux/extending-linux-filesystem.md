---
title: Extending Linux File System
description: How-to guide for extending linux file systems following the resizing of a volume. 
published: true
date: 2022-07-08T17:16:56.094Z
tags: aws, ebs, fs, filesystem
editor: markdown
dateCreated: 2022-07-08T17:16:56.094Z
---

# Extending File Systems 
- [Relevant AWS Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/recognize-expanded-volume-linux.html)
- [Stackoverflow - xfs_growfs - centos](https://stackoverflow.com/questions/26305376/resize2fs-bad-magic-number-in-super-block-while-trying-to-open)

Scenario: I launched an ec2 without specifying a size for the attached EBS volume resulting in only 8GB of storage. I manually increased the volume size of the block device via the EC2 console. Once complete, the file system itself must be extended to consume the newly available space. 

## Centos

Most of these steps will be applicable to future sections of this page. 

Verify system and type (ext4, etc) for each volume: 

```
df -hT
```

Centos (AWS EBS?) uses xfs by default:

`/dev/xvda1     xfs       8.0G  6.7G  1.3G  84% /`

Check whether the volume has a partition that must be extended, use the lsblk command to display information about the NVMe block devices attached to your instance.

```
lsblk
---
xvda     202:0    0  100G  0 disk 
└─xvda1 202:1    0    8G  0 part /
```

In this example, our xvda block (root volume) has 100GB (new size) of available space and we want to extend the xvda1 partition to use all that available space. Note: the partition still reflects the original size of the volume. 

```
volume
 └─partition
```

For volumes that have a partition, such as the root volume shown in the previous step, use the growpart command to extend the partition. Notice that there is a space between the device name and the partition number.

```
sudo growpart /dev/nvme0n1 1
```

Confirm with `lsblk`; you should see that the partition you referenced with the `partition number (1)` has expanded to take up the available block space. 

`df -h` will reveal that the file system still reflect the original volume size (8GB in this case).

You'll need to extend the file system. Note that these commands will differ depending on the type of file system being used. 

```
sudo xfs_growfs -d /
```

DONE! Verify with `df -h`

