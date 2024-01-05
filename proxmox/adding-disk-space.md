---
title: Adding Disk Space to a Proxmox VM
description: How to increase the disk space of a Proxmox VM. 
published: true
date: 2024-01-05T18:39:01.934Z
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

