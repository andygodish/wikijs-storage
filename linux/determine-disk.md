---
title: Determine Disk Type
description: Guide on how to determine if you underlying disk is an SSD or and HHD.
published: true
date: 2022-09-04T20:06:16.828Z
tags: linux, storage
editor: markdown
dateCreated: 2022-09-04T20:03:02.126Z
---

# Determine Disk Type

- [Reference Article](https://ostechnix.com/how-to-find-if-the-disk-is-ssd-or-hdd-in-linux/)

## Method 1

```
cat /sys/block/sda/queue/rotational
```

If the output is 0, you are using an SSD. If the output is 1, it's an HHD. 

Note the `/sda`, make sure this is the correct block. 