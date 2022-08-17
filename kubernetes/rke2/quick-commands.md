---
title: RKE2 Quick Commands
description: Quick snippets for copying and pasting commands for RKE2 clusters.
published: true
date: 2022-08-17T13:01:37.079Z
tags: kubernetes, rke2
editor: markdown
dateCreated: 2022-08-17T03:18:29.409Z
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

## Modify rke2.yaml to Reflect Server as Indicated by tls-san Configuration

This is used for accessing the kubeapi-server via a machine other than local host. 

### Wait for rke2 to bootstrap:

```
echo "Waiting for rke2 config file to exist.."
while [[ ! -f /etc/rancher/rke2/rke2.yaml ]]; do
  sleep 2
done
```

### Make a copy for modification & Modify:

- RKE2 configuration should be set to include `write-kubeconfig-mode: "0644"`
- Make sure your `cp_lb_host` variable is set

```
cp /etc/rancher/rke2/rke2.yaml /tmp/rke2.yaml
sed -i -e "s/127.0.0.1/${cp_lb_host}/g" /tmp/rke2.yaml
```

