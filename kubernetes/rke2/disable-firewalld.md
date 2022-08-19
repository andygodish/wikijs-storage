---
title: Disable Firewalld
description: Firewalld should be disabled for RKE2 clusters.
published: true
date: 2022-08-18T04:13:22.695Z
tags: kubernetes, rke2, firewalld
editor: markdown
dateCreated: 2022-08-17T03:18:14.648Z
---

# Firewalld & RKE2

[Reference Docs](https://docs.rke2.io/known_issues/#firewalld-conflicts-with-default-networking)

Firewalld conflicts with RKE2's default `Canal` (Calico + Flannel) networking stack. To avoid unexpected behavior, firewalld should be disabled on systems running RKE2.

## Disabling Firewalld

Use systemctl.

- Runs by default on Centos Stream 8

```
systemctl stop firewalld.service
systemctl disable firewalld.service
```