---
title: Network Troubleshooting
description: Network Troubleshooting Basics
published: true
date: 2022-08-10T19:07:31.955Z
tags: linux, networking, network
editor: markdown
dateCreated: 2022-08-10T19:07:31.955Z
---

# Linux Network Troubleshooting	

## Commandline Tools

### Ping

Pretty straight forward. ICMP echo requests are being sent and returned. Note the round trip time (rtt) statistics provided in the output of a ping command. 

```
--- 1.0.0.1 ping statistics ---
7 packets transmitted, 7 received, 0% packet loss, time 6010ms
rtt min/avg/max/mdev = 13.619/14.132/15.063/0.557 ms
```

### TCPDump

Say you want to capture icmp packets on a particular interface (ens18) on a host. You can open a terminal and listen for icmp packets on that host using `tcpdump`. 

```
sudo tcpdump -n icmp -i ens18
```

The output will provide a little more detail than received during a ping request. 

### Traceroute

Determine the route you are taking to get to a destination. 


