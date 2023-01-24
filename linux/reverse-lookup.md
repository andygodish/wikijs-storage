---
title: Reverse Lookup
description: The antithesis of nslookup.
published: true
date: 2023-01-24T04:45:50.610Z
tags: linux, command line
editor: markdown
dateCreated: 2023-01-24T04:45:50.610Z
---

# Reverse Lookup

A reverse lookup can be performed using the `host` command followed by the ip address of a server.

```
host 192.168.2.86
```

```
# output

86.2.168.192.in-addr.arpa domain name pointer ubuntu-docker-traefik.
```

Interesting to see that the ip address octets are reversed. 

---

This is the antithesis of an `nslookup`.

```
nslookup blog.lan.andyg.io
```

```
# output

Server:         192.168.96.1
Address:        192.168.96.1#53

Non-authoritative answer:
Name:   blog.lan.andyg.io
Address: 192.168.2.86
```

