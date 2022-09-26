---
title: Powershell Script Execution
description: Security settings that allow the execution of PS scripts. 
published: true
date: 2022-09-25T20:16:05.587Z
tags: powershell, windows
editor: markdown
dateCreated: 2022-09-25T20:16:05.587Z
---

# Powershell Script Execution

Powershell will complain if a script you are trying to run is not made executable. The following will introduce major security violations as it allows the execution of all scripts on a machine. To do this, run the following in PS:

```
Set-ExecutionPolicy Unrestricted
```

TODO: 
- Research how to make individual or groups of files executable. 