---
title: Scaling Replicas with Compose
description: Scaling Replicas with docker-compose. 
published: true
date: 2023-01-02T16:10:46.886Z
tags: docker
editor: markdown
dateCreated: 2023-01-02T16:10:46.886Z
---

# Scaling Containers with Compose

Say you have a service launched from this docker-compose.yaml: 

```
version: '3'

services:

  ...

  whoami:
    # A container that exposes an API to show its IP address
    image: traefik/whoami
    labels:
      - "traefik.http.routers.whoami.rule=Host(`whoami.docker.localhost`)"
```

You can control the amount of deployed replicas with the `--scale` flag like so:

```
docker-compose up -d --scale whoami=2
```