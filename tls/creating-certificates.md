---
title: Creating TLS Certificates
description: Writing down this process so I don't forget next time I have to do this for work. 
published: true
date: 2022-12-16T15:06:37.688Z
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

#### Subject Alternative Names

A SAN or subject alternative name is a structured way to indicate all of the domain names and IP addresses that are secured by the certificate.

This is what the SANs would like in the certificate details:

```
SAN 1: DNS Name=example.com
SAN 2: DNS Name=www.example.com
SAN 3: DNS Name=example.net
SAN 4: DNS Name=mail.example.com
SAN 5: DNS Name=support.example.com
SAN 6: DNS Name=example2.com
SAN 7: IP Address=93.184.216.34
SAN 8: IP Address= 2606:2800:220:1:248:1893:25c8:1946
```

#### Public CAs 

Example: Digicert is stored on client devices by default. 

These trusted public CAs can be viewed on your device. 

## Self Signed Certificates

On an internal network, you would establish a *private CA* that would sign your certificates so you can install them on your servers. 

The CA would also need to be installed to the *trusted root CA store* off each client using your network.

## Creating Your Own Certificate Authority

First, generate a new RSA key. This will be the private key of the CA cert, keep it safe. Because it is so sensitive, it is encrypted with `aes` and should also include a password. 

```
openssl genrsa -aes -out ca-key.pem 4096
```



 
