---
title: Terragrunt Installation and Configuration
description: Installation Guide for Terragrunt
published: true
date: 2022-09-03T19:18:41.250Z
tags: terraform, terragrunt
editor: markdown
dateCreated: 2022-09-03T19:18:41.250Z
---

# Terragrunt Installation and Configuration

- [Installation Docs](https://terragrunt.gruntwork.io/docs/getting-started/install/)
- [Releases Page](https://github.com/gruntwork-io/terragrunt/releases)

## Install

Navigate to the release page and look for the latest release. 

Find your architecture-specif release asset, right click and copy the link address. Run the following command to download on Linux:

```
curl -LO https://github.com/gruntwork-io/terragrunt/releases/download/v0.38.9/terragrunt_linux_amd64
```

Rename the binary to `terragrunt`, make it an executable, and place it on your path. 

```
mv terragrunt_linux_amd64 terragrunt
azure-terraform-base chmod u+x terragrunt
sudo mv terragrunt /usr/bin/
```

## Determine CPU Architecure

- [Reference StackOverflow Post](https://askubuntu.com/questions/133111/how-can-i-check-if-my-cpu-is-amd64-compatible)

Linux/WSL:

```
lscpu
```

> For Intel systems, the Vendor ID is - GenuineIntel and for AMD - AuthenticAMD.

> But they are compatible. It is a bit of an historical artifact in that AMD was the creator of the 64 bit 'long' mode, and later Intel matched that creation.
But naming often refers to any 64 bit binary as an AMD/64bit format, usually with the label 'AMD64'.


