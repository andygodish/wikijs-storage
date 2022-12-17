---
title: Redis Basics
description: Just the basics of using Redis.
published: true
date: 2022-12-17T17:41:14.748Z
tags: storage, redis, data
editor: markdown
dateCreated: 2022-12-17T17:34:14.058Z
---

# Redis Basics	

## Bootstrapping with Docker

Start a new docker network for the service. This will allow you to build additional applications on the same network. 

```
docker network create redis
```

The default port for redis is `6379`. Because redis is running in the same network as your future application, there is no need to expose it other than to communicate with the server from your local machine.

```
docker run -it --rm --name redis --net redis -p 6379:6379 redis:6.2-alpine
```

