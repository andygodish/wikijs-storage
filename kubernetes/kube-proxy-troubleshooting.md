---
title: Kube-proxy Troubleshooting
description: Notes on troubleshooting the kube-proxy.
published: true
date: 2022-08-09T15:57:19.451Z
tags: kubernetes, kube-proxy
editor: markdown
dateCreated: 2022-08-09T15:57:19.451Z
---

# Kube-Proxy Troubleshooting

- [K8s troubleshooting docs](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/#is-the-kube-proxy-working)

After verification that your application services are functioning properly, you'll want to move up the chain and focus on the service proxying mechanism in its entirety. 

You can confirm that the kube-proxy is running on the node by running the following ps command: 

```
ps auxw | grep kube-proxy
```

Look at the logs and rule out the following error indicating an obvious failure to contact the master: 

```
I1027 22:14:54.038238    5063 proxier.go:429] Not syncing iptables until Services and Endpoints have been received from master
```

## Conntrack

> One of the possible reasons that kube-proxy cannot run correctly is that the required conntrack binary cannot be found. This may happen on some Linux systems, depending on how you are installing the cluster, for example, you are installing Kubernetes from scratch. If this is the case, you need to manually install the conntrack package (e.g. sudo apt install conntrack on Ubuntu) and then retry.


## RKE2 

In RKE2, the kube-proxy will run as a pod in your cluster in the `kube-system` namespace.

Logs can be viewed using a label: 

```
k logs -n kube-system -l component=kube-proxy -f
```

