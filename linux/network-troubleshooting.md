---
title: Network Troubleshooting
description: Network Troubleshooting Basics
published: true
date: 2022-08-10T19:24:36.012Z
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

#### Chaining Options Together

Use the `and` option.

```
tcpdump -v -i ens18 src 192.168.1.170 and dst 1.0.0.1 and udp
```

`-v` for verbose can be used to gather additional information in your output. For example, if you wanted to see the ttl on each hop. 

### Traceroute

- Needs to be installed on centos-stream

Used to determine the route you are taking to get to a destination. 

```
traceroute 1.1

 1  unifi.localdomain (192.168.1.1)  0.673 ms  0.610 ms  0.563 ms
 2  cm-1-acr10.cosprings.co.denver.comcast.net (96.120.13.193)  30.790 ms  30.972 ms  30.747 ms
 3  ae-258-1257-rur201.cosprings.co.denver.comcast.net (96.110.194.69)  17.976 ms  18.225 ms  18.450 ms
 4  24.124.155.205 (24.124.155.205)  17.324 ms  17.314 ms  17.481 ms
 5  96.216.22.45 (96.216.22.45)  20.573 ms  20.544 ms  20.219 ms
 6  be-36041-cs04.1601milehigh.co.ibone.comcast.net (96.110.43.253)  20.164 ms be-36011-cs01.1601milehigh.co.ibone.comcast.net (96.110.43.241)  16.876 ms  16.833 ms
 7  be-3202-pe02.910fifteenth.co.ibone.comcast.net (96.110.38.118)  22.413 ms be-3402-pe02.910fifteenth.co.ibone.comcast.net (96.110.38.126)  13.763 ms  13.714 ms
 8  50.208.232.118 (50.208.232.118)  14.341 ms  14.042 ms  14.261 ms
 9  one.one.one.one (1.0.0.1)  13.260 ms  24.330 ms  23.484 ms
```

The first column can be said as, for example (first line), the first time we send out the packet with a time to live (ttl) of 1, and the rest of the line is the response coming back from the first router. 

Traceroute will by default send out udp packets on linux/mac. Windows uses icmp by default. 

If you wanted to send icmp packets using traceroutes, you can use `-I` flag. 

```
sudo traceroute -I 1.1
```

You'll see the same hops using ICMP, only you will see timeouts for all ICMP replies until you've reachec the final destination (1.0.0.1 in this case). 






