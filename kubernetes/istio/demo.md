---
title: Istio Demo
description: Outlining the basics of Istio 1.16 from the demo provided in their documentation.
published: true
date: 2022-12-23T05:43:00.976Z
tags: kubernetes, istio, service mesh
editor: markdown
dateCreated: 2022-12-23T05:43:00.976Z
---

# Istio Demo

- The [docs](https://istio.io/latest/docs/setup/getting-started/)

This demo utilizes the `istio install` method. Production environments may require the use of helm or the istio operator upon further investigation. 

First up, download the release: `curl -L https://istio.io/downloadIstio | sh -
`. You'll need to maintain the binary associated with the version pulled in. 

## Installation

Change into the downloaded Istio directory and add the `istioctl` binary to your path. Using that binary, install istio to your cluster with a [profile](https://istio.io/latest/docs/setup/additional-setup/config-profiles/).

```
istioctl install --set profile=demo -y
```

You need this label for sidecar injection to take place within a namespace: 

```
kubectl label namespace default istio-injection=enabled
```


