---
title: DNS
description: How to work with DNS on a local Windows machine. 
published: true
date: 2022-08-19T22:31:30.530Z
tags: windows, dns
editor: markdown
dateCreated: 2022-08-19T22:31:30.530Z
---

# DNS on Windows Machine

- [Resource Article](https://sufi.io/linux-etcresolv-conf-equivalent-in-windows-8-to-add-domain-suffixes/)

Check and configure your DNS server by using your network adaptor found in the Network Connections app. You'll see a page of network connections such as your wi-fi network or your VPN connection. 

Right click a network followed by Propteries. Then, use the Networking tab at the top of the dialog and double click the IPV4 record found in the main table. This will reveal your DNS server addresses. 

### Powershell

```
Get-DnsClientServerAddress
```

