---
title: SELINUX
description: Notes and quick commands for SELINUX
published: true
date: 2022-12-03T17:32:23.521Z
tags: linux, security, selinux
editor: markdown
dateCreated: 2022-12-03T17:32:23.521Z
---

# SELINUX

- [reference article](https://linuxhint.com/how-do-i-set-selinux-to-permissive-mode/)

In my development environment, I am usually deploying to centos 8 VMs that are bootstrapped with SELinux set to enforcing by default. 

## Set SULinux to Permissive

```
sudo setenforce 0
```

To verify the status, run `sudo setenforce 0`.





