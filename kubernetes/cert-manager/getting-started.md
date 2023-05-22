---
title: Cert-Manager
description: Getting started with a cert-manager deployment. 
published: true
date: 2023-05-22T19:03:01.805Z
tags: kubernetes, tls, cert-manager
editor: markdown
dateCreated: 2023-05-22T18:59:28.040Z
---

# Cert-Manager

Walkthrough on installing and configuring a Let Encrypt certificate for a service deployed behind an Nginx Ingress Controller in an `Azure AKS cluster`. 

- [Cert-Manger Installation](https://cert-manager.io/docs/installation/helm/
- [Nginx Ingress Controller Installation](https://cert-manager.io/docs/tutorials/acme/nginx-ingress/)

## Helm Chart

I like to pull in remote charts for use with GitOps workflows provided by Argo or Flux. See my wiki on helm pulls for information on how to do this.


Add the repos (requires helm 3); Jetstacks is for cert-manager:
```
helm repo add jetstack https://charts.jetstack.io
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
```

## Installing Cert-Manager
 
Base Installation command with the addition of the cert-manager CRDs as indicated by the --set flag: 

```
helm install cert-manager ./ --namespace cert-manager \
  --create-namespace \
  --set installCRDs=true
```

## Install Nginx Ingress Controller

By default, the ingress controller will prompt Azure, based on the role assigned to an SPN created to manage the deployment of the cluster, to create a loadbalancer service type. This will be given a public IP address that can be mapped to your domain. 

The point is to have a single point of ingress for HTTPS. TLS termination will take place at the ingress controller. Ingress resources associated with each service will determine how traffic is routed beyond the controller. 

Helm install:

```
helm install ingress-nginx ./ --create-namespace \
  --namespace ingress-nginx \
  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/health
```

## Cert-Manager CRDs

### Issuer

In the reference article linked above, a "staging" Issuer was created to prevent exceeding the limits set by Let's Encrypt. For this wiki, I am only reviewing the prod Issuer as indicated by the `server` file in the yaml below:

- staging can be defined with this configuration: `server: https://acme-staging-v02.api.letsencrypt.org/directory`

```
apiVersion: cert-manager.io/v1
kind: Issuer
metadata:
  name: letsencrypt-prod
spec:
  acme:
    # The ACME server URL
    server: https://acme-v02.api.letsencrypt.org/directory
    # Email address used for ACME registration
    email: andy@andyg.io
    # Name of a secret used to store the ACME account private key
    privateKeySecretRef:
      name: letsencrypt-prod
    # Enable the HTTP-01 challenge provider
    solvers:
    - http01:
        ingress:
          class:  nginx
```

I'm using cloudflare for DNS. I did not need to coinfigure any other records (ex: TXT) to get this to work for me. 

## Ingress Configuration

Example ingress resource that 

```
kind: Ingress
metadata:
  name: andygio
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/use-regex: "true"
    cert-manager.io/issuer: "letsencrypt-prod"
spec:
  ingressClassName: nginx
  tls:
  - hosts:
    - andyg.io
    secretName: andygio-tls
  rules:
  - host: "andyg.io"
    http:
      paths:
      - pathType: Prefix
        path: "/"
        backend:
          service:
            name: andygio
            port:
              number: 8080
```

A cople points:

- Note the annotations for configuring nginx and cert-manager. Make sure the targeted issuer exists. 
- The `secretName` defines the secret which cert-manager will create --- this will contain the `.crt` and `.key` data.

Once you create the ingress, Cert-manager will perform several steps that involve the creation of additional CRDs. I found that monitoring the associatet `Certificate` and `CertificateRequest` resource via *describe* commands was enough to help me troubleshoot through the issues I encountered. 

That's it, check your hosted site and verify you have a valid TLS certificate.



