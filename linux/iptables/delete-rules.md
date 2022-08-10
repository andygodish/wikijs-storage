---
title: Delete Rules
description: Deleting rules.
published: true
date: 2022-08-10T22:36:51.430Z
tags: linux, iptables, delete
editor: markdown
dateCreated: 2022-08-10T22:19:47.536Z
---

# Delete Rules

Use the following command to delete an IP table rule in a chain

sudo iptables -D CHAIN #

```
sudo iptables -D INPUT 1
```

where,
- `-D` delete
- `1` line-number for the rule you are deleting (--line-numbers)