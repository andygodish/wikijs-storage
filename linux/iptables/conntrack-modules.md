---
title: Conntracking States and Modules
description: Intro to loading modules in iptables rules and the conntracking module.
published: true
date: 2022-08-18T04:13:48.587Z
tags: linux, iptables, conntrack
editor: markdown
dateCreated: 2022-08-17T03:18:58.638Z
---

# Modules	

Load modules as part of adding rules in an iptables command using the `-m` flag. 

## Conntrack Module

Connection tracking states is handled by linux, but in order to take advantage of it, a couple iptables rules need to be added to the INPUT chain of the Filter table. 

```
sudo iptables -A INPUT -j ACCEPT -m conntrack --cstate ESTABLISHED,RELATED
sudo iptables -A OUTPUT -j ACCEPT -m conntrack --cstate ESTABLISHED,RELATED
```

You are now allowing "established" traffic.

