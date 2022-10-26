---
title: Helm Demo
description: Helm demonstration. 
published: true
date: 2022-10-26T14:25:27.152Z
tags: kubernetes, helm
editor: markdown
dateCreated: 2022-10-25T03:05:03.526Z
---

# Helm Demo

## Prerequisites

- Kubernetes cluster (RKE2 single node)
- One point of ingress for use by the Nginx Ingress Controller (for demonstration of web app deployment)
- [Helm isntalled](https://helm.sh/docs/intro/install/)

## Simple Web Application

Using kubernetes manifests, demonstrate the amount of yaml that goes into each manifest. In this example, you will need the following k8s resources: 

- Deployment
- Service
- Ingress

## Create a New Helm Chart

```
helm create chart
```

Remove the scaffolded template files and demonstrate simply just moving the previous manifest files into that directory. 

## Install the Chart Locally

From the new chart's directory, create a new release and use it to demonstrate a simple upgrade by makeing changes to the Chart.yaml, followed by a `helm upgrade` on the same release. 

