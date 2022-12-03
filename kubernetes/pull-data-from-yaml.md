---
title: Pull Data From Yaml Output
description: Getting data output from a yaml using jsonpath. 
published: true
date: 2022-12-03T18:59:14.001Z
tags: kubernetes, kubectl
editor: markdown
dateCreated: 2022-12-03T18:59:14.001Z
---

# Pulling Data from YAML

Often, I need to fetch a datafield from a secret or configmap. Once I know the key used in the field, I can use kubectl + jsonpath to fetch the value. If it's a secret, I can then pipe the output and base64 decode it.

```
kubectl get secret --namespace wordpress wordpress -o jsonpath="{.data.wordpress-password}" | base64 -d
```