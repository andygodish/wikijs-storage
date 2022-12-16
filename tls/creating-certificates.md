---
title: Creating TLS Certificates
description: Writing down this process so I don't forget next time I have to do this for work. 
published: true
date: 2022-12-16T16:12:22.266Z
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

### Private Key

First, generate a new RSA key. This will be the private key of the CA cert, keep it safe. Because it is so sensitive, it is encrypted with `aes` and should also include a password. 

```
openssl genrsa -aes -out ca-key.pem 4096
```

### Generate CA Certificate

Generate an x509 certificate using the private key as input from the previous step. 

```
openssl req -new -x509 -sha256 -days 365 -key ca-key.pem -out ca.pem
```

#### Reading a Certificate

- Use `-text` for human readable format.
- There is a `CA:TRUE` line for certs that are for CAs 

```
openssl x509 -in ca.pem -text
``` 

## Issuing Certificates

The target addess you are navigating to in your browser is the important part to keep in mind when generating a certificate.

### Generate another Private Key

No need to protect this key with a password.

```
openssl genrsa -out key.pem 4096
```

### Generate a Certificate Signing Request

So you're not actually generating a certificate at this point, just a request for the CA to sign one for you. 

- the CN is not important

```
openssl req -new -sha256 -subj "/CN=yourcn" -key key.pem -out cert.csr
```

Adding in your DNS names/IP addresses to the certificate comes next. 

To do this, you create a config file containing your SANs.

```
echo "subjectAltName=DNS:*.dns.record,IP:192.168.1.123" >> extfile.cnf
```

## Generate the Certificate from the CSR

Important players:
- the CA and its key
- the CSR as your input + configuration

The CAcreateserial parameter creates an incrementing serial number - this is important when generating multiple certificates.

```
openssl x509 -req -sha256 -days 365 -in cert.csr -CA ca.pem -CAkey ca-key.pem -out cert.pem -extfile extfile.cnf -CAcreateserial
```

## Building the Certificate Chain

The ca.pem and the cert.pem must be combined to create the full certificate chain. You just combine the outputs into one file. 

```
cat cert.pem > fullchain.pem

cat ca.pem >>./fullchain.pem
```

## Upload the Certificate on Your Servers

This will be slightly different depending on the application you are using. Follow the documentation. Typically, you will be uploading the private key and the certificate chain. 