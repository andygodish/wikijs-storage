---
title: Setting Up a Dev Cluster
description: Breakdown of how I am using RKE2 to run a development cluster in my homelab. 
published: true
date: 2022-12-23T22:52:07.617Z
tags: kubernetes, rke2, homelab, development
editor: markdown
dateCreated: 2022-12-21T16:46:51.163Z
---

# Setting Up a Dev Cluster

## Kubernetes Cluster

**K8s Distribution:** RKE2
**Infrastructure:** Proxmox VMs / Centos Stream 
**Remote Cluster Access:** Kube-VIP
**Storage Class:** local-path (rancher) 

### Initial Server Node

> **Don't forget to change the hostname of your VM**

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

Be sure to reload NetworkManager, `systemctl reload NetworkManager`.

Start and enable rke2-server,

```
systemctl --now enable rke2-server
```

#### Kube-VIP

I already wrote a [demo wiki](https://github.com/andygodish/wikijs-storage/blob/main/kubernetes/kube-vip/kubevip-demo.md) for the initial set up of `kube-vip`. Check it out. 

Refer to the [kube-vip documentation](https://kube-vip.io/) as well. You'll want to adjust the version TAG variable to the [latest release](https://github.com/kube-vip/kube-vip/releases) as well. Doublecheck the interface variable too.

This process installs kube-vip as a daemonset in the cluster. You should see a single kube-vip pod running almost immediately following the installation command. Test this by pinging the address used for the vip, `192.168.2.64`.

#### Accessing the Cluster Remotely

Move the `rke2.yaml` kubeconfig file created during the rke2 installation process to a tmeporary directory and updated the server to reflect your *kube-vip* instead of localhost. 

```
cp /etc/rancher/rke2/rke2.yaml /tmp/rke2.yaml
sed -i -e "s/127.0.0.1/${VIP}/g" /tmp/rke2.yaml
```

**From your development machine**, copy the temporary rke2.yaml yaml file and set its path equal to your `KUBECONFIG` env variable. You should now be able to access the cluster via kubectl.  

### Joining Additional Server Nodes

For the remaining two server nodes, you'll first want to repeat the rke2 configuration step by creating the `/etc/rancher/rke2/config.yaml` file with updated information in the tls-san block pertaining to the new server nodes. 

Additionally, you will need to include keys for the server and the join token. The latter of which can be retrived from your initial server node at `/var/lib/rancher/rke2/server/token`.

```
cat <<EOF >> /etc/rancher/rke2/config.yaml
server: https://192.168.2.64:9345
token: <join-token>
EOF
```

Repeat the following steps outlined in the *Initial Server Node* section: 
- Disable firewalld if required
- Configure NetworkManager (reload)
- Download rke2 installation binary
- Enable and start the rke2-server service

### Joining Worker Nodes

The configuration file for a simple worker nodes just needs to specify a server and join token,

```
mkdir -p /etc/rancher/rke2
cat <<EOF > /etc/rancher/rke2/config.yaml
server: https://192.168.2.64:9345
token: <join-token>
EOF
```

Repeat the following steps outlined in the *Initial Server Node* section: 
- Disable firewalld if required
- Configure NetworkManager (reload)

#### Download the Agent Installation Binary

```
curl -sfL https://get.rke2.io | INSTALL_RKE2_CHANNEL=stable INSTALL_RKE2_TYPE="agent" sh -
```

Finally, start and enable the `rke2-agent` service.

## Storage Class

- [Rancher's Local Path Povisioner](https://github.com/rancher/local-path-provisioner)

This storage class dynamically applies a hostpath volume and claim and saves you the hassle of doing it manually. Follow the installation steps outlined in the GitHub Readme above. Or,

```
kubectl apply -f https://raw.githubusercontent.com/rancher/local-path-provisioner/v0.0.23/deploy/local-path-storage.yaml
```



