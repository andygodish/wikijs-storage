---
title: K3s Quick Commands
description: Copy and past command for quickly working with k3s on the nodes themseleves.
published: true
date: 2024-05-28T14:45:08.028Z
tags: kubernetes, k3s
editor: markdown
dateCreated: 2024-05-28T14:44:24.106Z
---

# Quick Commands

##Useful Environment Variables
```
export CONTAINER_RUNTIME_ENDPOINT=unix:///run/k3s/containerd/containerd.sock
export CONTAINERD_ADDRESS=/run/k3s/containerd/containerd.sock
export PATH=/var/lib/rancher/k3s/bin:$PATH
export KUBECONFIG=/etc/rancher/k3s/k3s.yaml
alias k=kubectl
```