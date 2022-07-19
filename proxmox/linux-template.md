---
title: Linux Template
description: How-to guide showing the basics of creating a template out of your a linux box.
published: true
date: 2022-07-19T22:54:11.514Z
tags: linux, proxmox, template
editor: markdown
dateCreated: 2022-07-19T21:54:15.978Z
---

# Proxmox Linux Template

Circle Back to this page and build out any additional sections pertaining to the specifics of various operating systems

## SSH Keys

Ssh host keys are unique to each machine and are located at `/etc/ssh`. Remove them and allow `cloud-init` to regenerate them for you in each new machine based on your template. 

```
sudo rm /etc/ssh/ssh_host_*
```

## Clear Machine ID

`/etc/machine-id` contains an id that also must be unique for each machine created from your template.

It is advised that you don't delete this (Possibley due to it being a symlink?), but rather resize it to 0.

```
sudo truncate -s 0 /etc/machine-id
```

Verify that `/var/lib/dbus/machine-id` is a symbolic link pointing to `/etc/machine-id`.

```
ls -l /var/lib/dbus/machine-id
```

If `/var/lib/dbus/machine-id` is not a symlink, you'll need to make create it.

```
sudo ln -s /etc/machine-d /var/lib/dbux/machine-id
```

## Additional Cleaning

TODO - add best practices to this section.

## Creating Template in Proxmox

- Shutdown machine
- Right click machine --> convert to template

### Remove Attachment to .iso for the Virtual Disk

This was presented as a best practice, and not a requirement. 

- Click Template --> Hardware --> CD/DVD Drive --> Edit --> Do not use any media
	- Hardware --> Add --> CloudInit Drive --> Storage selection --> Create
    	- Cloud Init --> make updates as needed (user+password, ssh pubkey, etc)
     
### Creating VMs 

To create a new VM based on this template, just right click the template and click clone. 

#### Update Hostname

```
sudo vi /etc/hostname
---
name-n
```

```
sudo vi /etc/hosts
---
127.0.1.1 name-n
```

Reboot your nodes following these updates. 

## Centos

I have a centos stream 8 box that I want to replicate so I can create an HA Kubernetes cluster on a small intel NUC. 

#### Search for a particular package

```
dnf search cloud-init
```

#### Clean Chache

[Reference Guide](https://www.techrepublic.com/article/linux-101-how-to-clean-the-dnf-and-apt-caches/)

Deletes cached information pertaining to updating and upgrading software packages, the data in these cahces can become corrupted.

Cache files generated from the repository metadata.

```
sudo dnf clean dbcache
```



```
sudo dnf clean all
```

## Ubuntu

[Reference Video](https://www.youtube.com/watch?v=t3Yv4OOYcLs&t=326s)

#### Search for a particular package

```
apt search cloud-init
```

#### Basic Clean Up 

- clean - clears chache of packages that may be outdated by the time you get around to buidling a machine from this template
- autoremove - removes orphaned packages

```
sudo apt clean
sudo apt autoremove
```





