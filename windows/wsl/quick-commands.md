---
title: WSL Quick Commands
description: A list of quick commands for wsl.
published: true
date: 2022-08-20T06:14:22.080Z
tags: windows, wsl
editor: markdown
dateCreated: 2022-08-18T03:29:11.418Z
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