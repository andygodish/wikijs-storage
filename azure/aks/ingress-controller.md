---
title: AKS Ingress Controller Configuration
description: AKS Ingress Controller Configuration
published: true
date: 2023-05-22T14:21:05.850Z
tags: kubernetes, aks, ingress-controller
editor: markdown
dateCreated: 2023-05-22T02:53:11.671Z
---

# AKS Ingress Controller Configuration

## NGINX Ingress Controller

```
NAMESPACE=ingress-nginx
```

- [Reference Article](https://learn.microsoft.com/en-us/azure/aks/ingress-basic?tabs=azure-cli)

This walkthrough will use the default values provded by the upstream helm chart. 

### Namespace

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
```

```
helm install ingress-nginx ingress-nginx/ingress-nginx \
  --create-namespace \
  --namespace $NAMESPACE \
  --set controller.service.annotations."service\.beta\.kubernetes\.io/azure-load-balancer-health-probe-request-path"=/healthz
```

This will result in the deployment of type LoadBalancer that should recieive a public IP from Azure within a minute of being created. 

You can then map an A record to the IP of the LoadBalancer provisioned by Azure. Now there is a single point of ingress for traffic to be routed in to your cluster. From there, an Ingress resource can be created for any number of services. For example: 

```
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: andygio
  annotations:
    nginx.ingress.kubernetes.io/ssl-redirect: "false"
    nginx.ingress.kubernetes.io/use-regex: "true"
spec:
  ingressClassName: nginx
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



