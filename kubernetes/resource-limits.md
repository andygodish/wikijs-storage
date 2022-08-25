---
title: Resource Limits
description: Information pertaining to resource quotas on kubernetes workloads. 
published: true
date: 2022-08-25T15:35:36.805Z
tags: kubernetes
editor: markdown
dateCreated: 2022-08-25T15:35:36.805Z
---

# Resources

By default, K8s assumes a request of `0.5 CPU` and `256Mi`. Modify these values in the resources stanza in your manifests. 

- 1m (millicore) is the lowest amount you can request
- 0.1 CPU == 100m
- 1 == 1 vCPU

Apparently there are also default limits set for containers in kubernetes, `1 CPU` and `512Mi`. 

You need to create a LimitRange specifying the default request and limit within a given namespace. 

```
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
```

K8s will throttle CPU so it can't use more CPU resources than its limit.  

A container can use more memory than its limit. If this happens constantly, the pod will eventually crash. 