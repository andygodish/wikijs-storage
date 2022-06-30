---
title: Backup Postgresql Database
description: Quick how-to on using pg_dump to backup a wikijs database running inside a Kubernetes pod. 
published: true
date: 2022-06-30T02:12:34.361Z
tags: kubernetes, wikijs, psql
editor: markdown
dateCreated: 2022-06-30T02:12:34.361Z
---

# Backup Postgres Database Running in K8s Pod

Manual step-by-step process for grabbing postgresql data from a kubernetes pod. 

This is being used for the purposes of maintaining a simple base set of configurations for my personal wiki js deployment. The rational for this is so that I can tear down and redeploy in an automated fashion at a later time. 

Following [these wikijs docs](https://docs.requarks.io/install/transfer), slightly translated to work with a Kubernetes deployment. 

## Prerequisites

A working kubernetes cluster with kubectl access. 

## Exec into Pod, Run pg_dump

```
kubectl exec -it wiki-postgresql-0 -n wiki -- bash
```

pg_dump comes packaged in the postgres image used by the wikijs helm chart. Create a backup of the database: 

```
pg_dump <database> -U <user> -F c > /tmp/wikibackup.dump
```
- You'll need to write this to a directory you have permissions to do so.
- You'll be promted for a password that is stored (base64 encoded) in a K8s secret by the wikijs helm chart.

## Use Kubectl to Copy the Backup.dump

```
kubectl cp namespace/pod:tmp/wikibackup.dump /local/path/wikibackup.dump
```
- You must name the destination file
