---
title: Persistence
description: Persist your iptables rules across server reboots.
published: true
date: 2022-08-11T15:25:19.266Z
tags: linux, iptables
editor: markdown
dateCreated: 2022-08-11T15:25:19.266Z
---

# Iptables Persistence

## iptables-persistent/iptables-services package

### Debian/Ubuntu

```
sudo apt install iptables-persistent
```

### RHEL/Centos

```
sudo dnf install iptables-services
```

### Save Rules

### Debian/Ubuntu

Use the `iptables-save` command to write the current rules to a .v4 or .v6 file. IPV6 rules can be saved using the `ip6tables-save` command.

```
sudo sh -c "iptables-save" > /etc/iptables/rules.v4
```

### RHEL/Centos

1. Make sure iptables-services is installed. 
2. Stop firewalld
3. Enable iptables service (this is why we stopped firewalld)
4. Run iptables save (different destination directory)

```
sudo sh -c "iptables-save" > /etc/sysconfig/iptables
```

Reboot and confirm the rules persisted. You will need to start the iptables service first. I'm not sure what the reprocussions are for running both iptables and firewalld. 