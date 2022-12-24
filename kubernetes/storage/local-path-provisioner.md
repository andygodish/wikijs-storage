---
title: Local Path Provisioner
description: Using Rancher's Local Path Provisioner storageclass.
published: true
date: 2022-12-24T20:03:42.111Z
tags: kubernetes, storage
editor: markdown
dateCreated: 2022-12-24T20:03:09.813Z
---

# Local Path Storage Class

## Installation

- [Rancher's Local Path Povisioner](https://github.com/rancher/local-path-provisioner)

This storage class dynamically applies a hostpath volume and claim and saves you the hassle of doing it manually. Follow the installation steps outlined in the GitHub Readme above. Or,

```
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.23/deploy/local-path-storage.yaml
```

Make it your default storageclass,

```
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

## Permission Issues

I ran into an issue when trying to install wikijs via a helm chart set to use the "local-path" storageclass. The Local Path Provisioner (LPP) spins up a helper pod that runs a busy box image and tries to create volumes on node's at the `/opt/local-path-provisioner/` directory. 

With SELinux set to enforcing, I get several selinux errors that will need to be looked into in the future. However, when set to permissive, I still get a permission error when the helper container tries to write to that directory. A quick, non-production, fix for this is to set the permissions on that directory to allow "all other users" the ability to write to those directories. Original permission for that directory upon installing LPP are `755`, `root:root`.

Run the following on all nodes being used to store PVs:

```
chmod 757 -R /opt/local-path-provisioner
```