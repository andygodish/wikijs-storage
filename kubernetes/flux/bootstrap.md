---
title: Bootstrapping Flux
description: Getting started with Flux.
published: true
date: 2023-03-13T22:08:24.830Z
tags: kubernetes, flux, gitops
editor: markdown
dateCreated: 2023-03-13T22:08:24.830Z
---

# Bootstrapping Flux

## Prerequisites

- Install the flux cli
- Kubernetes cluster that meet the `flux check --pre` requirements
- Github Account

## Bootstrap using GitHub

Setting your personal access token to the env variable, `GITHUB_TOKEN`, will allow the flux bootstrap process to pick it up automatically.

```
export GITHUB_USER=andygodish
export GITHUB_TOKEN=xxx_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
```

The next step will create a repo for you assuming it doesn't already exist in your GH account. 

```
flux bootstrap github \
--owner=$GITHUB_USER \
--repository=lb \
--branch=main \
--path=app-cluster \
--personal
```

Verify by checking that the `flux-system` namespace has been created in your cluster. 


