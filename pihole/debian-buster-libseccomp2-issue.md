---
title: Fix Docker Incompatibility with Libseccomp2
description: Issue with libseccomp2 library not being up to date on debian docker package repos. 
published: true
date: 2022-12-14T03:48:01.433Z
tags: 
editor: markdown
dateCreated: 2022-12-14T03:48:01.433Z
---

# Fix Docker Incompatibility with Libseccomp2
- [Reference Article](https://docs.linuxserver.io/faq#option-2)
- [GitHub Issue](https://github.com/linuxserver/docker-jellyfin/issues/71#issuecomment-733621693)

When attempting to launch a pihole container via a docker-compoase configuration, I observed the container cashlooping with the following errors present in the logs: 

```
s6-linux-init-hpr: fatal: unable to reboot(): Operation not permitted
s6-svscan: warning: unable to iopause: Operation not permitted
s6-svscan: warning: executing into .s6-svscan/crash
s6-svscan crashed. Killing everything and exiting.
```

Option 2 worked for me (Option 1 did not :( ):

```
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 04EE7237B7D453EC 648ACFD622F3D138
echo "deb http://deb.debian.org/debian buster-backports main" | sudo tee -a /etc/apt/sources.list.d/buster-backports.list
sudo apt update
sudo apt install -t buster-backports libseccomp2
```

