---
title: Certificate Inspection
description: Short write-up on openssl basics. 
published: true
date: 2022-11-03T14:30:25.958Z
tags: linux, ssl/tls, openssl
editor: markdown
dateCreated: 2022-11-03T14:30:25.958Z
---

# Certificate Inspection

Openssl can be used to inspect certificates used by webservers. 

## Using s_client to Fetch Live Certificates

```
openssl s_client -connect google.com >> google.crt
```

## Using x.509 to inspect a Certificate

```
openssl x.509 -in <crt> --noout
```

Where `--noout` will prevent the hashed certificate from being outputted at the bottom of the returned text. 