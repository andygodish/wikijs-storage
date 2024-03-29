---
title: Bootstrapping Flux
description: Getting started with Flux.
published: true
date: 2024-03-29T15:47:59.949Z
tags: kubernetes, flux, gitops
editor: markdown
dateCreated: 2023-10-08T22:30:44.835Z
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

The next step will create a new repo based on the `--repository` tag using the `GITHUB_TOKEN` set as an env variable in the previous step. See [docs](https://fluxcd.io/flux/installation/bootstrap/github/) for permission details. 

```
flux bootstrap github \
  --token-auth \
  --owner=$GITHUB_USER \
  --repository=flux-fleet \
  --branch=main \
  --path=clusters/my-cluster \
  --personal
```

Verify by checking,

- that the `flux-system` namespace has been created in your cluster
- the `flux-system` directory has been created in your new repo
- the deploy key for your flux app-cluster has been created


