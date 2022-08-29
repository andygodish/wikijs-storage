---
title: Sudoers
description: How-to guide for configuring users as sudoers.
published: true
date: 2022-08-29T20:26:54.502Z
tags: linux, sudo, usermod
editor: markdown
dateCreated: 2022-08-17T03:15:21.021Z
---

# Sudo User Configuration

The objective of this how to guide is to configure a user to be able to run sudo commands without being prompted for a password. 

## Centos

### Add User to Wheel Group 

[Reference Guide](https://linuxize.com/post/how-to-add-user-to-sudoers-in-centos/)

By default in Red Hat operating systems, users who are part of the wheel group have sudo access by default. In this example, we are configuring the *centos* user.

```
sudo usermod -aG wheel centos
```
- In Ubuntu, the group is simply `sudo` instead of wheel. All other configuration steps are the same. 


You can now run `sudo su` and switch to the root user. At this point, you will be prompted to enter a password. 

### Disable Password Prompt

[Reference Guide](https://www.cyberciti.biz/faq/how-to-sudo-without-password-on-centos-linux/)

```
sudo visudo
---
# Add this line to the file:
centos ALL=(ALL) NOPASSWD:ALL
```

No more password required. You could also configure the *wheel* group to have sudo access without the need for a password. 
