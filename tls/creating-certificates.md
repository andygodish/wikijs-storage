---
title: Creating TLS Certificates
description: Writing down this process so I don't forget next time I have to do this for work. 
published: true
date: 2022-12-16T03:13:16.457Z
tags: ssl/tls, tls
editor: markdown
dateCreated: 2022-12-16T03:13:16.457Z
---

# Creating TLS Certificates

## TLS Protocol Basics

TLS certs have two main functions: encypting the connection & validating trust between client and server. 

SSL Certificates are written in x509 standard (format for defining public key certificates).

![tls_handshake.png](/images/tls_handshake.png)