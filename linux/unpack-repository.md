---
title: Unpacking a Package Repository
description: How to unpack package repositories pulled in from the internet.
published: true
date: 2022-12-14T19:02:38.370Z
tags: linux, dpkg, package-repository
editor: markdown
dateCreated: 2022-12-14T19:02:38.370Z
---

# Unpacking a Package Repository

## Debian/Ubuntu

### Puppet Server Example

At the beginning of the installation process for the Puppet Server, you are required to *enable the puppet platform respository* by using `wget` to pull down the repository associated with your os.

```
wget https://apt.puppet.com/puppet-release-jammy.deb
```

This will result in the package being downloaded to your PWD. From there, you need to unpack the *installed package* using `dpkg`. 

```
dpkg -i <installed_package>.deb
```

Now, subsequent `apt` commands will work against this new package repository. 