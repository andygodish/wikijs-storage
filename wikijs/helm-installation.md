---
title: Helm Installation
description: Install Wikijs on a Kubernetes cluster using Helm.
published: true
date: 2022-06-30T05:26:57.905Z
tags: helm, kubernetes, wikijs
editor: markdown
dateCreated: 2022-06-29T15:02:56.155Z
---

# Wikijs Installation

Install wiki js on a Kubernetes cluster with Helm. 

## Prerequisites
- Existing Kubernetes cluster
- Helm

# Installation

## Create a namespace for your wikijs instance

`kubectl create namespace wiki`

## Add the requarks wikijs helm repository to your local machine

`helm repo add requarks https://charts.js.wiki`

This repo is stored on the github page for the app itself:
https://github.com/requarks/wiki/tree/main/dev/helm 

See the values.yaml file in the repo listed above for more information on configuration.

I am also maintaining a github repo where I can make potential modifications to various versions of the helm chart and package them as my own releases: 
https://github.com/andygodish/wikijs-chart

## Install 

I'm using a values.yaml file to inject my desired configuration settings into the helm release. This will eventually be stored in its own repo following best practices for k8s configuration. 

`helm install -n wiki wiki requarks/wiki -f values.yaml`

Simple **values.yaml** example: 

*A couple things to note:*

- I am bringing my own tls cert and private key and telling the ingress resource (created by a helm chart template) to use an existing secret containing that data, called tls-ingress-wiki

- Persistence is disabled in this example. You'll need to have a storeage class available for the dynamic provisioning of a PV. You're wiki deployment pods will fail to deploy due to a missing volume if you try to install this chart without a valid storageclass.
```
ingress:
  enabled: true
  className: nginx
  hosts: 
  - host: wiki.example.com
    paths: 
    - path: /
      pathType: Prefix 
  tls:
  - hosts:
    - wiki.example.com
    secretName: tls-ingress-wiki
postgresql:
  enabled: true
  persistence:
    enabled: false
```




