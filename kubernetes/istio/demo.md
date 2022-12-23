---
title: Istio Demo
description: Outlining the basics of Istio 1.16 from the demo provided in their documentation.
published: true
date: 2022-12-23T05:51:50.550Z
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

### Installing the Sample Application

Install the *bookinfo* application with the following command:

```
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```

**NOTE** - The `istio-init` containers will crashloop when running on an OS running SELinux. [This article](https://www.suse.com/support/kb/doc/?id=000020241) solved the issue for me. TLDR, run this on your worker nodes:

```
cat >/etc/modules-load.d/istio-iptables.conf <<EOF
br_netfilter
nf_nat_redirect
xt_REDIRECT
xt_owner
EOF
```

