---
title: BigBang Quickstart Guide
description: https://repo1.dso.mil/big-bang/bigbang/-/blob/master/docs/guides/deployment-scenarios/quickstart.md
published: true
date: 2023-10-09T20:04:45.601Z
tags: kubernetes, devops, bigbang
editor: markdown
dateCreated: 2023-10-09T19:34:36.880Z
---

# Bigbang Quickstart Guide

Bullet points pertaining to my walkthough of the BigBang quickstart guide provided by Platform1. 

- [Repo](https://repo1.dso.mil/big-bang/bigbang/-/blob/master/docs/guides/deployment-scenarios/quickstart.md)

### Rocky9 

#### User Permissions

The equivalent of this,  

```
# Allow members of group sudo to execute any command, no password
%sudo   ALL=(ALL:ALL) NOPASSWD:ALL
```

Would be, 

```
## Same thing without a password
%wheel  ALL=(ALL)       NOPASSWD: ALL
```

#### Docker Installation

- [Reference Article](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-rocky-linux-8)