---
title: Quick Commands
description: Quick commands and tools for iptables.
published: true
date: 2022-08-18T04:14:14.024Z
tags: linux, iptables
editor: markdown
dateCreated: 2022-08-17T03:19:42.105Z
---

# Iptables Quick Commands

*Convenient alias that shows verbose output, numberic cidr output, and line-numbers*

```
alias ipt=iptables -L -v -n --line-numbers
```

##### DNS

- port 53 TCP & UDP (two OUTPUT chain rules)

