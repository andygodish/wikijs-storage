---
title: Resource Limits
description: Information pertaining to resource quotas on kubernetes workloads. 
published: true
date: 2022-08-26T03:19:03.633Z
tags: kubernetes
editor: markdown
dateCreated: 2022-08-25T15:35:36.805Z
---

# Resource Limits and Requests

An apparent memory leak was oberserved in one of our API server workloads that eventually cascaded into multiple crashlooping pods eventually bringing down a cluster node. The pods and kubelet process were observered to be comsuming a large pertentage of the node's available CPU. Limiting the amount of CPU a pod is able to use was the obvious solution to this problem.

In K8s there is a resource of of LimitRange. When applied to a namespace, all pods sharing that namespace will inherit the resource configurations of that LimitRange. Consider a container requesting 1 millicore of CPU and 256Mi of memory with a cap of 1 CPU and 2Gi of memory: 

```
# pod.spec.containers[*].resources:

  resources:
      limits:
        cpu: "1"
        memory: 2Gi
      requests:
        cpu: 1m
        memory: 256Mi
```

A LimitRange would automatically apply this configuration to each pod container at the time of scheduling. A LimitRange that applies the above confiuration would look like this: 

```
apiVersion: v1
kind: LimitRange
metadata:
  name: limit-range
spec:
  limits:
  - default:
     memory: 2Gi
     cpu: 1
    defaultRequest:
      memory: 256Mi
      cpu: 0.25
    type: Container
```

Additional configuration details can be found in the [k8s documentation](https://kubernetes.io/docs/concepts/policy/limit-range/).

You can override these "default" values by setting a request on your deployment manifest directly. You'll need to delete any existing pods in that replicaset in order for the new requests configurations to be applied. 

---

## More Info

- 1m (millicore) is the lowest amount of CPU a container can request.
- K8s will throttle CPU so a resource can't use more CPU resources than its requested limit.  
- A container can use more memory than its requested limit, but will eventually crash if its limit is exceeded for an extended period of time. 