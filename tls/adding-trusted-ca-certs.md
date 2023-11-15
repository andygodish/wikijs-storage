---
title: Adding Trusted CA Certs
description: How to add trusted CA certificates on various Operating Systems. 
published: true
date: 2023-11-15T22:48:19.534Z
tags: linux, ssl/tls, wsl
editor: markdown
dateCreated: 2023-11-15T22:48:19.534Z
---

# Adding Trusted CA Certs

## Example - Root CA not trusted in WSL

In the infrastructure I use for work, I have access to a non-prod web application that I can access via my Windows machine both via a browser and via curl in Powershell. The web server returns a certificate chain similar to this: 

- root ca (private ca)
	- intermediate (also internally generated)
  		- domain (end-entity) certificate
   
When I open an Ubuntu terminal within WSL, I see this error when running the same `curl` command: 

```
curl: (60) SSL certificate problem: self-signed certificate in certificate chain
More details here: https://curl.se/docs/sslcerts.html

curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```

I found that,

> The Windows Subsystem for Linux (WSL) maintains its own separate set of trusted root certificates, independent from the Windows host. This means that even if a certificate is trusted in Windows, it won't automatically be trusted within the WSL environment. This is why you're seeing the SSL certificate error when using curl in Ubuntu on WSL.

To resolve, 

1. Copy the Certificate to WSL: First, you need to get the ROOTCA certificate file into your WSL environment. You can copy it from Windows or directly download it within WSL.

```
cp /mnt/c/Users/username/Documents/<exported-cert>.crt /tmp/
```

2. Add the Certificate to the Trusted Store:

a. Place the certificate file (usually with a .crt or .pem extension) in the /usr/local/share/ca-certificates/ directory. You might need to create this directory if it doesn't exist.
  
```
sudo mkdir -p /usr/local/share/ca-certificates/
sudo cp /tmp/<exported-cert>.crt /usr/local/share/ca-certificates/
```

b. Update the list of trusted certificates. This command will add the new certificate to the system's list of trusted CAs.

```
sudo update-ca-certificates
```

Verify by running the `curl` command again. 

You should be able to see the `<exported-cert>.pem` in the `/etc/ssl/certs` directory. NOTE: the `update-ca-certificates` command appears to change the extension from `.crt` to `.pem` automatically. 

