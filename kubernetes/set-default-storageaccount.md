---
title: Set Default StorageAccount
description: How to set the default storage account.
published: true
date: 2022-12-03T18:37:44.708Z
tags: kubernetes, storage
editor: markdown
dateCreated: 2022-12-03T18:37:44.708Z
---

# Setting the Default Storage Account

In my development clusters I typically use Rancher's local-path storageclass for quick provisioning of a pv on the host machine without the need to manually create a hostPath claim.

```
kubectl patch storageclass local-path -p '{"metadata": {"annotations":{"storageclass.kubernetes.io/is-default-class":"true"}}}'
```

This annotation will add the `(default)` marker found when listing all storageclasses in your cluster.