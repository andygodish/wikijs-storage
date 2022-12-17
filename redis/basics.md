---
title: Redis Basics
description: Just the basics of using Redis.
published: true
date: 2022-12-17T20:42:00.926Z
tags: storage, redis, data
editor: markdown
dateCreated: 2022-12-17T17:34:14.058Z
---

# Redis Basics	

## Bootstrapping with Docker

```
# TLDR Entire Command:
docker run -it --rm --name redis --net redis -v C:\Users\Andrew.P.Godish\Desktop\config:/etc/redis -v redis:/data/ -p 6379:6379 redis:6.2-alpine redis-server /etc/redis/redis.conf

docker run -it --rm --name redis --net redis -v ${PWD}/config:/etc/redis -v redis:/data/ -p 6379:6379 redis:6.2-alpine redis-server /etc/redis/redis.conf
```

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

There are two modes of persistence (see docs):
- RDB mode
- AOF mode

#### RDB Mode

In the config file, activate the line, `dbfilename dump.rdb`. You can edit the file location as you see fit. 

#### AOF Mode

In the config file, activate the line, `appendonly yes`. Then, set a filename to have redis write its operations tp, `appendfilename "appendonly.aof"`.

---

You can run both modes simultaneously by activating them both in the configuration. 

To persist, create a docker volume called redis and map it to the /data/ dir in your container:

```
docker volume create redis

# Corresponding volume:
-v redis:/data/
```






