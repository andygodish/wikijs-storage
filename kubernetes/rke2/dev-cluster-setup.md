---
title: Setting Up a Dev Cluster
description: Breakdown of how I am using RKE2 to run a development cluster in my homelab. 
published: true
date: 2022-12-21T20:16:24.868Z
tags: kubernetes, rke2, homelab, development
editor: markdown
dateCreated: 2022-12-21T16:46:51.163Z
---

# Setting Up a Dev Cluster

## Kubernetes Cluster

**K8s Distribution:** RKE2
**Infrastructure:** Proxmox VMs / Centos Stream 
**Remote Cluster Access:** Kube-VIP
**Service Mesh:** Istio 

### Initial Server Node

#### config.yaml

A couple things to note: 

- The intent is to use the Istio service mesh, so we won't need a traditional ingress controller and can set the default nginx controller to disabled
- The `192.168.2.64` ip in the `tls-san` array will be the VIP used in future the kube-vip configuration - above that is the DNS I am using locally for that VIP

```
mkdir -p /etc/rancher/rke2

cat <<EOF > /etc/rancher/rke2/config.yaml
disable: rke2-ingress-nginx
write-kubeconfig-mode: "0644"
node_label: 
- "env=dev"
tls-san:
- server-0
- server-0.lan.andyg.io
- dev-rke2.lan.andyg.io
- 192.168.2.64
EOF
```

Use the installation script from the [rke2 docs](https://docs.rke2.io/upgrade/manual_upgrade?_highlight=channe#release-channels) to install rke2-server from the stable channel: 

```
curl -sfL https://get.rke2.io | INSTALL_RKE2_CHANNEL=stable sh -
```

Since I am running on Centos Stream, I will need to disable firewalld per the 
[rke2 documentation](/kubernetes/rke2/config-network-manager)https://docs.rke2.io/known_issues?_highlight=firewalld).

```
systemctl --now disable firewalld
```

Additionally, when running NetworkManager, you need to make sure it is configured to ignore calico/flannel related network interfaces. [Docs](https://docs.rke2.io/known_issues?_highlight=firewalld#networkmanager).

```
touch /etc/NetworkManager/conf.d/rke2-canal && \
cat <<EOF >> /etc/NetworkManager/conf.d/rke2-canal
[keyfile]
unmanaged-devices=interface-name:cali*;interface-name:flannel*
EOF
```

Start and enable rke2-server,

```
systemcctl --now enable rke2-server
```

