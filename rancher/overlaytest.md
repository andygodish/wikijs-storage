---
title: Overlay Network Test
description: Rancher provided script for troubleshooting the overlaynetwork of a k8s cluster. 
published: true
date: 2022-07-11T17:59:10.743Z
tags: kubernetes, rancher, networking
editor: markdown
dateCreated: 2022-07-11T17:59:10.743Z
---

# Rancher Overlay Network Test 
[Rancher Documentation](https://rancher.com/docs/rancher/v2.5/en/troubleshooting/networking/)

I placed the yaml file for this deployment in my own Github repo. It can be quickly applied to a k8s cluster with the following command: 

```
kubectl apply -f https://raw.githubusercontent.com/andygodish/rancher-overlaytest/main/overlaytest.yaml
```

