---
title: Istio Demo
description: Outlining the basics of Istio 1.16 from the demo provided in their documentation.
published: true
date: 2022-12-24T20:08:43.896Z
tags: kubernetes, istio, service mesh
editor: markdown
dateCreated: 2022-12-23T05:43:00.976Z
---

# Istio Demo

- The [docs](https://istio.io/latest/docs/setup/getting-started/)

This demo utilizes the `istio install` method. Production environments may require the use of helm or the istio operator upon further investigation. 

First up, download the release: `curl -L https://istio.io/downloadIstio | sh -
`. You'll need to maintain the binary associated with the version pulled in. 

## Installation

Change into the downloaded Istio directory and add the `istioctl` binary to your path. Using that binary, install istio to your cluster with a [profile](https://istio.io/latest/docs/setup/additional-setup/config-profiles/).

```
istioctl install --set profile=demo -y
```

You need this label for sidecar injection to take place within a namespace: 

```
kubectl label namespace default istio-injection=enabled
```

### Modify The Istio-Ingressgateway Service

If you're not using an external loadbalancer, edit the `istio-ingressgateway` service deployed during the istio installation to be of type NodePort instead of LoadBalancer.

### Installing the Sample Application

Install the *bookinfo* application with the following command:

```
kubectl apply -f samples/bookinfo/platform/kube/bookinfo.yaml
```

**NOTE** - The `istio-init` containers will crashloop when running on an OS running SELinux. [This article](https://www.suse.com/support/kb/doc/?id=000020241) solved the issue for me. TLDR, run this on your worker nodes:

```
modprobe br_netfilter 
modprobe nf_nat_redirect 
modprobe xt_REDIRECT 
modprobe xt_owner
```

To load on reboot of the node, run:

```
cat >/etc/modules-load.d/istio-iptables.conf <<EOF
br_netfilter
nf_nat_redirect
xt_REDIRECT
xt_owner
EOF
```

Verify the deployment with this curl command: 

```
kubectl exec "$(kubectl get pod -l app=ratings -o jsonpath='{.items[0].metadata.name}')" -c ratings -- curl -sS productpage:9080/productpage | grep -o "<title>.*</title>"
```

### Apply the Ingress Gateway and Virtual Service

```
kubectl apply -f samples/bookinfo/networking/bookinfo-gateway.yaml
```

You can run `istioctl analyze` for further validation.

---

### Quick Commands for Accessing Demo Apps

```
export INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="http2")].nodePort}')
export SECURE_INGRESS_PORT=$(kubectl -n istio-system get service istio-ingressgateway -o jsonpath='{.spec.ports[?(@.name=="https")].nodePort}')
export INGRESS_HOST=$(kubectl get po -l istio=ingressgateway -n istio-system -o jsonpath='{.items[0].status.hostIP}')
export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT
echo "http://$GATEWAY_URL/productpage"
```

## More about Gateways and Virtual Services

These are Istio CRDs that allow the management of inbound and outbound traffic for the service mesh. 

> Gateway configurations are applied to **standalone Envoy proxies** that are running at the edge of the mesh, rather than sidecar Envoy proxies running alongside your service workloads.

Gateways allow access to Istio's *traffic routing*.

A Gateway is bound to a `VirtualService` to specify routing for the Gateway. Routing rules for external traffic are then configured on the `VirtualService`.

### Sample Gateway

```
apiVersion: networking.istio.io/v1beta1
kind: Gateway
metadata:
  ...
spec:
  selector:
    istio: ingressgateway
  servers:
  - hosts:
    - '*'
    port:
      name: http
      number: 80
      protocol: HTTP
```

The `selector` used in the Gateway maps the Gateway resource to the `istio-ingressgateway` service deployed in the `istio-system` namespace. 

The above example allows external traffic from *all* hosts into the mesh on port 80.

## Kiali Dashboard

The addons directory contains a Kiali deployment.

```
kubectl apply -f samples/addons
kubectl rollout status deployment/kiali -n istio-system
```

Once installed, launch the dashboard with `istioctl dashboard kiali`.

Open the webapp in your browser and navigate to the *graph* sectin on the left tab. Switch to the namespace you wish to view trace data for. Generate some trace data with the following for loop:

```
for i in $(seq 1 100); do curl -s -o /dev/null "http://$GATEWAY_URL/productpage"; done
```