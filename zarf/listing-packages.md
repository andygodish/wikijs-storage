---
title: Listing Zarf Packages in a Cluster
description: zarf package list
published: true
date: 2024-07-27T04:54:19.548Z
tags: zarf
editor: markdown
dateCreated: 2024-07-27T04:54:19.548Z
---

# Listing Zarf Packages in a Cluster

Running `zarf package list` in the context of the k8s cluster you are connected to results in a list of the packages deployed into that cluster.

![zarf-package-list.png](/images/zarf-package-list.png)

My initial thought was that the `zarf init` package deployed a CRD. This is not the case. 

## Source Code

The function called by this command can be found [here](https://github.com/zarf-dev/zarf/blob/main/src/pkg/cluster/zarf.go#L25). 

The function returns a list of **secrets** in the zarf namespace that contain a label key of `package-deploy-info` as defined wy the `ZarfPackageInfoLabel` constant. 

