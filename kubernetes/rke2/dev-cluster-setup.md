---
title: Setting Up a Dev Cluster
description: Breakdown of how I am using RKE2 to run a development cluster in my homelab. 
published: true
date: 2022-12-21T17:09:56.306Z
tags: kubernetes, rke2, homelab, development
editor: markdown
dateCreated: 2022-12-21T16:46:51.163Z
---

# Setting Up a Dev Cluster

## Kubernetes Cluster

**K8s Distribution:** RKE2
**Infrastructure:** Proxmox VMs / Centos Stream 
**Remote Cluster Access:** Kube-VIP
**Service Mesh:**Istio 

### Initial Server Node

#### config.yaml

