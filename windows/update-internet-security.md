---
title: Windows Internet Security
description: Adjusting Windows Internet Security Settings
published: true
date: 2022-09-25T19:52:21.001Z
tags: windows
editor: markdown
dateCreated: 2022-09-25T19:52:21.001Z
---

# Windows Internet Security

## Scenario I

- [Reference Article](https://techcult.com/fix-your-current-security-settings-do-not-allow-this-file-to-be-downloaded/)

I was working on a windows server running in my home lab. As part of a tutorial, I needed to download an github repository containing a PS script. Downlaoding a zip of the repo via url resulting in an error prompting me to adjust the machine's internet security policies. The following steps were taken. 

Windows Key + R then type 

```
inetcpl.cpl
```

Scroll down until you find Downloads section, and set all download options to `Enabled`.