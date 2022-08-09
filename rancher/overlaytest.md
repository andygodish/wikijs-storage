---
title: Overlay Network Test
description: Rancher provided script for troubleshooting the overlaynetwork of a k8s cluster.
published: true
date: 2022-08-09T15:28:04.353Z
tags: kubernetes, rancher, networking
editor: markdown
dateCreated: 2022-08-09T01:11:47.749Z
---

# Rancher Overlay Network Test 
- [Rancher Documentation](https://rancher.com/docs/rancher/v2.5/en/troubleshooting/networking/)
- [My Repo](https://github.com/andygodish/rancher-overlaytest)

The overlay network test will run a ping test between containers on all hosts.

## Deploy Daemonset

I placed the yaml file for this deployment in my own Github repo. It can be quickly applied to a k8s cluster with the following command: 

```
kubectl apply -f https://raw.githubusercontent.com/andygodish/rancher-overlaytest/main/overlaytest.yaml
```

## Run the Overlay Test Script

I placed the script in the same github repo for quick access. 

```
curl -sfL https://raw.githubusercontent.com/andygodish/rancher-overlaytest/main/run.sh | sh -
```

### Fail Scenarios

There may be output that indicates a node cannot communicate via a route on the overlaynetwork to another node. 

- Check to make sure all required ports are open on the affected nodes. 

## Cleanup

```
kubectl delete -f https://raw.githubusercontent.com/andygodish/rancher-overlaytest/main/overlaytest.yaml
```
