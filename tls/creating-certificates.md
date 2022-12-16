---
title: Creating TLS Certificates
description: Writing down this process so I don't forget next time I have to do this for work. 
published: true
date: 2022-12-16T05:46:27.104Z
tags: ssl/tls, tls
editor: markdown
dateCreated: 2022-12-16T03:13:16.457Z
---

# Creating TLS Certificates

## TLS Protocol Basics

TLS certs have two main functions: encypting the connection & validating trust between client and server. Once accomplished, data can be transferred.  

SSL Certificates are written in x509 standard (format for defining public key certificates).

![tls_handshake.png](/images/tls_handshake.png)

The certificate always contains a public key (shared with anyone trying to access the server). Additionally, the server will house a corresponding private key that remains on the server used to decrypt the client's messages. 

The certificate also contins the server's identity. 

