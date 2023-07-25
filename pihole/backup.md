---
title: Backup Solution Pihole
description: Process guide for backing up Pihole in my homelab.
published: true
date: 2022-12-31T20:53:07.587Z
tags: nas, pihole, homelab
editor: markdown
dateCreated: 2023-07-24T20:53:07.587Z
---

# Back Up Solution

This write-up primarily deals with the backing up of the relevant directtories in the filesystem used by the pihole conttainer. In my lab, I use a product called [gravity](https://github.com/vmstan/gravity-sync) sync that backs up my primary pihole instance to a second instance of pihole running on another server. 

This process utilizes an NFS mounnt from the primary pihole server to my qnap NAS. 

## Relevant Files

Pi-hole stores its configuration and data on the local filesystem in two different directories outlined here by my `docker-compose.yaml`:

```
 volumes:
   - '$PWD/etc-pihole/:/etc/pihole/'
   - '$PWD/etc-dnsmasq.d/:/etc/dnsmasq.d/'
```

These two directories will be backed up to a shared volume on my NAS at `/share/pihole`

### Mounting QNAP Shared Folder

- [Reference Article](https://jarasyola.blogspot.com/2017/02/how-to-mount-qnap-shared-folder-in-linux.html)

```
sudo mount -t nfs <nas.ip>:/share/pihole /mnt/qnap_nas/pihole
```


