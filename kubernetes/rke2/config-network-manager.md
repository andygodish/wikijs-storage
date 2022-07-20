---
title: Network Manager Configuration
description: Recommended configuration settings for Network Manager per the rke2 documentation. 
published: true
date: 2022-07-20T15:24:56.468Z
tags: linux, redhat, networkmanager
editor: markdown
dateCreated: 2022-07-20T15:24:56.468Z
---

# RKE2 NetworkManager Configuration

[Reference Docs](https://docs.rke2.io/known_issues/#networkmanager) 

- Network manager is configured to run by default on Centos Stream 8

NetworkManager manipulates the routing table for interfaces in the default network namespace where many CNIs, including RKE2's default, create veth pairs for connections to containers. This can interfere with the CNI’s ability to route correctly. As such, if installing RKE2 on a NetworkManager enabled system, it is highly recommended to configure NetworkManager to **ignore calico/flannel related network interfaces**. 

In order to do this, create a configuration file called `rke2-canal.conf` in `/etc/NetworkManager/conf.d` with the contents:

```
sudo vi /etc/NetworkManager/conf.d/rke2-canal.conf
---
# file content:

[keyfile]
unmanaged-devices=interface-name:cali*;interface-name:flannel*
```

## If RKE2 is not Installed

```
sudo systemctl reload NetworkManager
```

## If RKE2 is Installed

Reboot the node for the changes to take effect. 

## Additional Services

In some operating systems like RHEL 8.4, NetworkManager includes two extra services called nm-cloud-setup.service and nm-cloud-setup.timer. These services add a routing table that interfere with the CNI plugin's configuration. Unfortunately, there is no config that can avoid that as explained in the issue. Therefore, if those services exist, they should be disabled and the node must be rebooted.

- These do not appear to be present on Centos Stream 8

