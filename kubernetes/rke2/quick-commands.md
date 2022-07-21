---
title: RKE2 Quick Commands
description: Quick snippets for copying and pasting commands for RKE2 clusters. 
published: true
date: 2022-07-21T16:11:08.367Z
tags: kubernetes, rke2
editor: markdown
dateCreated: 2022-07-21T16:11:08.367Z
---

# RKE2 Quick Commands

## Useful Environment Variables

```
export CONTAINER_RUNTIME_ENDPOINT=unix:///run/k3s/containerd/containerd.sock
export CONTAINERD_ADDRESS=/run/k3s/containerd/containerd.sock
export PATH=/var/lib/rancher/rke2/bin:$PATH
export KUBECONFIG=/etc/rancher/rke2/rke2.yaml
alias k=kubectl
```