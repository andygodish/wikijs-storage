---
title: Basic Installation
description: Steps outlining installation of Rancher
published: true
date: 2022-10-05T20:09:20.858Z
tags: kubernetes, rancher
editor: markdown
dateCreated: 2022-10-05T20:09:20.858Z
---

# Basic Installation

Basic installation steps for a Rancher instance running in various environments. 

## Cattle-system Namespace

```
kubectl create ns cattle-system
```

## Cert-manager

Needed for the following certification options:
- Rancher Generated Certs
- Let's Encrypt

In this example, we will use the default Rancher Generated certs, which will require this value configuration in the Rancher helm installation: `ingress.tls.source=rancher`. NOTE: this can be omitted as it is the default option.

```
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --set installCRDs=true\
  --version v1.7.1
```

## Install Rancher

```
helm install rancher rancher-<CHART_REPO>/rancher \
  --namespace cattle-system \
  --set hostname=rancher.lan \
  --set bootstrapPassword=admin
```