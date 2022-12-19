---
title: Redis Cluster
description: Working with replication and clustering for Redis. 
published: true
date: 2022-12-19T15:58:06.784Z
tags: clustering, redis, ha, replication
editor: markdown
dateCreated: 2022-12-19T15:58:06.784Z
---

# Redis Cluster

## Replication 

### Master 

Configured to replicate data to other replicas.

### Replicas

Configured to read data from the master. 

Persistence is key. If the master data gets wiped, the replicas will copy down the empty database. 

---

To demonstrate replication, I want to deploy three instances of redis in docker containers with a minimal set of configurations (config file),

```
# Master node
protected-mode no
port 6379
masterauth password
requirepass password
```

```
# Worker nodes
protected-mode no
port 6379
slaveof redis-0 6379
masterauth password
requirepass password
```
