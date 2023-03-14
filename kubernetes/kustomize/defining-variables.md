---
title: Defining Variables
description: Defining variables in a kustomization.yaml.
published: true
date: 2023-03-14T15:00:31.608Z
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