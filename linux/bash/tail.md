---
title: Tail
description: Tail command and arguments.
published: true
date: 2022-12-24T19:11:48.144Z
tags: bash, linux, scripting
editor: markdown
dateCreated: 2022-12-24T19:11:48.144Z
---

# Tail

## Examples

Piping output of a large file to tail so that you can see the last 100 lines of the file:

```
cat /var/logs/messages | tail -n 100
```