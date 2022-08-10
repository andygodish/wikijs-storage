---
title: CoreDNS
description: CoreDNS basics.
published: true
date: 2022-08-10T15:13:55.339Z
tags: kubernetes, coredns
editor: markdown
dateCreated: 2022-08-10T15:10:17.794Z
---

# CoreDNS	

- [Reference Article](https://blog.opstree.com/2020/06/16/a-closer-look-at-coredns/)

Execing into pods will reveal an `/etc/resolv.conf` entry pointing to the IP address (typically ending in .10) of your coreDNS service resource. 

#### DNS names for Pods

> Like I said above that we change the entry of /etc/hosts to centralised DNS server. Well, it is right but partially. DNS does not do the entry of pods as we do by editing /etc/hosts file in pods (Format: <pod_name> IP). Instead, it creates a new hostname by replacing dots into dashes in the IP address of pods like hostname 10-244-2-5 (Format: hostname   IP). Refer figure 1d and look at the entry of DNS.
  
This is actually not done by default, but apparently can be turned on via a configuration setting in the Corefile.

The actual coreDNS default hostnames for pods and services looks like this: 

###### Format: 

For services: **svcname.namespace.type.rootDomain**
For pods: **hostname.namespace.type.rootDomain**

###### Example:  

For services: **test-service.default.svc.cluster.local**
For pods: **10-244-2-5.default.pod.cluster.local**

In testing a ping to simply `10-244-2-5.default.pod` resolved successfully. Does this have something to do with ndots options (/etc/resolv.conf)? 


  
#### Location of Corefile
  
`/etc/coredns/Corefile`

This is injected as a configmap for decoupling purposes. 

Each line item in the Corefile represents a configured plugin. Custom plugins can also be written. 

> Let’s talk about kubernetes plugin, Using the kubernetes plugin, CoreDNS will read zone data from a Kubernetes cluster. It implements the spec defined for Kubernetes DNS-Based service discovery

Take a look at the various options found in the reference article linked above. 

#### Kubelet Involvement

> The next step for the pods is to point to coreDNS IP address for DNS resolution by specifying nameserver in resolv.conf file. But, what address should it be?
Well, you don’t need to care about this because DNS entries have been handled by the kubelet component.

Take a look at the kubelet configuration settings (`ps aux | grep kubelet`) to see the --cluster-dns configuration. 


  
  