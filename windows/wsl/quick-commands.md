---
title: WSL Quick Commands
description: A list of quick commands for wsl.
published: true
date: 2022-11-29T22:10:14.212Z
tags: windows, wsl
editor: markdown
dateCreated: 2022-11-02T20:37:03.496Z
---

# WSL 

## Check Running VMs on Hyper-V 

Note: I am not 100% sure if wsl uses Hyper-V

```
wsl -l -v
```

You'll also see docker desktop running.

## Get the IPV4 of a VM (PS)

- [StackExchange Post](https://superuser.com/questions/1586386/how-to-find-wsl2-machines-ip-address-from-windows)

```
wsl -- ip -o -4 -json addr list eth0 `
| ConvertFrom-Json `
| %{ $_.addr_info.local } `
| ?{ $_ }
```

## Copy from Windows Mounted Drive

- [Reference Article](https://ridicurious.com/2018/10/18/2-ways-to-copy-files-from-windows-10-to-windows-sub-system-for-linux/)

Say you want to copy a file you downloaded in your windows os. Access it via the following path structure:

```
cp /mnt/c/Users/Andrew.P.Godish/Downloads/vr-infrastructure-actions-runner.2022-09-28.private-key.pem ~/.ssh
```

## Access WSL Filesystem

From a PowerShell instance, change into a WSL directory:

```
cd \\wsl.localhost\Ubuntu-22.04\home\andy\
```