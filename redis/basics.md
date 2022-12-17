---
title: Redis Basics
description: Just the basics of using Redis.
published: true
date: 2022-12-17T18:08:58.938Z
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

### Configuration

Check out the documentation pertaining to the raw github content for each major release version of redis. You'll find a complete list of commented out configuration options to use.

- [Redis 6.2](https://raw.githubusercontent.com/redis/redis/6.2/redis.conf)

Update the configuration file to meet your needs, then mount the configuration within the container using a Docker volume. 

- in this example, I am mounting an entire directory called config into the redis contianer. 

`-v ${PWD}/config:/etc/redis`

You can then run the *docker run* command with an entrypoint of `redis-server /etc/redis/redis.conf`.

### Security

Don't expose redis to public traffic as it does not have a password to connect by default. Update the password in your configuration file by editing the line, `requirepass <your-password-here>`.

#### TLS 

This is an optional featured introduced in 6.0 for situations where redis is not running within a private network and must be exposed to traffic. 

### Persistence

By default, Redis stores its data in memory. You need to mount that data somewhere to allow for the container to be destroyed/recreated. 


