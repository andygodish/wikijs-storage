---
title: Change IPV4
description: Quick how-to on changing the local IPV4 address on a windows server.
published: true
date: 2022-08-23T04:12:22.830Z
tags: networking, windows
editor: markdown
dateCreated: 2022-08-23T04:12:22.830Z
---

# Change IPV4

- [Reference Article](https://docs.microsoft.com/en-us/troubleshoot/windows-server/networking/change-ip-address-network-adapter)

Use the Control Panel to navigate to Network Connections. From there you need to select a Network Adapter. 

In the case of VMs running on a hypervisor, this will be a virtual ethernet interface. 

Right click the interface to the LAN and click properties. 

Click on the IPV4 checkbox in the main form box. Enter your IPV4 address where indicated. 


