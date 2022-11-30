---
title: Verify Server Port Connection
description: Basic usage of Netcat
published: true
date: 2022-11-30T19:26:32.412Z
tags: linux, networking, netcat
editor: markdown
dateCreated: 2022-11-30T19:26:32.412Z
---

# Verify Server Port Connection	

I was working through a tutorial that involved spinning up a MS SQL container running locally via docker. I was able to use Netcat to verify that it was running like so: 

```
nc -zv localhost 1433
```

