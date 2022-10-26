---
title: Mapping a Network Drive
description: How-to map a network drive on a linux machine.
published: true
date: 2022-10-26T22:19:54.056Z
tags: linux, nfs
editor: markdown
dateCreated: 2022-10-26T22:19:54.056Z
---

# Mapping a Network Drive

A raspberry pi running Raspbian Buster was used to mount a network drive from a QNAP NAS device running in my lab. 

## Create a New Mount

The goal is to use NFS to mount a drive used for file storage onto a single node k3s cluster so that I can leverage an NFS storage class for my applications running on the cluster. 

First, create the mount point on the local machine (pi). 

```
sudo mkdir /mnt/DIR
```

Where `DIR` corresponds to a shared folder on a volume on a QNAP storage pool. 

## Add Mount Point to /etc/fstab

Add the following (adjust for security purposes/future needs):

```
//192.168.1.55/DIR    /mnt/Chief      cifs    credentials=/path/to/password.txt,iocharset=utf8    0       0
```

Note the insecure nature of the credentials portion of the above entry. 

## Mount Drives

The following will mount the drives present in your fstab file. 

```
sudo mount -a
```

