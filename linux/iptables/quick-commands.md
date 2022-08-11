---
title: Quick Commands
description: Quick commands and tools for iptables.
published: true
date: 2022-08-11T15:05:23.649Z
tags: linux, iptables
editor: markdown
dateCreated: 2022-08-11T15:05:23.649Z
---

# Iptables Quick Commands

*Convenient alias that shows verbose output, numberic cidr output, and line-numbers*

```
alias ipt=iptables -L -v -n --line-numbers
```

##### DNS

- port 53 TCP & UDP (two OUTPUT chain rules)

