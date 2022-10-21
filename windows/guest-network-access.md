---
title: Allowing Guest Network Access
description: How to enable guest network access for mapping network drives on a Windows machine.
published: true
date: 2022-10-21T14:25:31.275Z
tags: networking, windows
editor: markdown
dateCreated: 2022-10-21T14:21:16.848Z
---

# Allow Guest Network Access

- [Reference Post](https://learn.microsoft.com/en-us/answers/questions/59309/you-cant-access-this-shared-folder-because-your-or.html)
- [YouTube Link](https://www.youtube.com/watch?v=vyatMj1Z2NQ&ab)

## Registry Editor

First, open up the `run` program, type `regedit` and hit enter.

Next, navigate to this path in the editor: 

```
Computer\HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\\LanmanWorkstation\Parameters
```

Click `AllowInsecureGuestAuth` and change the value data to 1. 

