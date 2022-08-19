---
title: Helm Upgrade
description: Upgrading a Rancher helm installation.
published: true
date: 2022-08-18T04:12:30.666Z
tags: rancher, upgrade
editor: markdown
dateCreated: 2022-08-17T03:16:46.180Z
---

# Rancher Upgrade 2.6.x --> 2.6.y
[Rancher doumentation outlining this process](https://rancher.com/docs/rancher/v2.6/en/installation/install-rancher-on-k8s/upgrades/).

## Update Helm Chart Repo

rancher-stable  https://releases.rancher.com/server-charts/stable --- helm chart generally used to install Rancher. 

```
helm repo update
```

## Fetch Helm Chart

```
helm fetch rancher-stable/rancher --version=v2.6.6
```

## Get Helm Values used in Initial Installation

```
helm get values rancher -n cattle-system
```

The outputted values will need to be apended to the following helm upgrade command.

## Upgrade Rancher

Use the --version flag to specify your target version. 

```
helm upgrade rancher rancher-stable/rancher \
  --namespace cattle-system \
  --set hostname=rancher.my.org
  --version=2.6.6
```
