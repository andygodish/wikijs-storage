---
title: Defining Variables
description: Defining variables in a kustomization.yaml.
published: true
date: 2023-03-14T15:02:15.751Z
tags: kubernetes, kustomization
editor: markdown
dateCreated: 2023-03-14T15:00:31.608Z
---

# Defining Variables

```
vars:
- name: REDISSERVICE
  objref:
    kind: Service
    name: redis
    apiVersion: v1
- name: REDISNAMESPACE
  objref:
    kind: Service
    name: redis
    apiVersion: v1
  fieldref:
    fieldpath: metadata.namespace
```

#### Using the variables in a base yaml

```
# deployment.yaml
  readinessProbe:
          httpGet:
            path: /index.html
            port: 8080
        env:
        - name: REDISHOST
          value: "$(REDISSERVICE).$(REDISNAMESPACE):6379"
        resources:
          requests:
            cpu: 25m
            memory: 50Mi
```