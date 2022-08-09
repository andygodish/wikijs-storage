---
title: Application Troubleshooting
description: Notes on troubleshooting K8s applications.
published: true
date: 2022-08-09T15:32:10.810Z
tags: kubernetes
editor: markdown
dateCreated: 2022-08-09T15:02:15.100Z
---

# Application Troubleshooting

## Endpoint Discovery

You'll want to verify that the services exposing your applications pods are properly discovering pod endpoints. 

An endpoint will be associated for each port exposed on each pod replica. For example, I am running two replicas of a Rancher deployment in my home lab where the pods are exposing port 80 and 443. Note the pod IPs in the following output: 

```
NAME                               READY   STATUS    RESTARTS      AGE     IP          NODE       NOMINATED NODE   READINESS GATES
rancher-5c4fb6b6bb-hf4rb           1/1     Running   0             14h     10.42.0.7   server-1   <none>           <none>
rancher-5c4fb6b6bb-sbhm9           1/1     Running   0             14h     10.42.2.9   server-3   <none>           <none>
```

Describing the service exposing this deployment should reveal 4 endpoints. Two for each pod (80, 443). 

```
k describe svc rancher -n cattle-system

...
NodePort:                 http  31756/TCP
Endpoints:                10.42.0.7:80,10.42.2.9:80   ##########
Port:                     https-internal  443/TCP
TargetPort:               444/TCP
NodePort:                 https-internal  32632/TCP
Endpoints:                10.42.0.7:444,10.42.2.9:444 ##########
Session Affinity:         None
...
```

There is an endpoint controller that is responsible for detecting these endpoints. [See the K8s documentation.](https://kubernetes.io/docs/tasks/debug/debug-application/debug-service/#does-the-service-have-any-endpoints)