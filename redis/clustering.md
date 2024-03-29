---
title: Redis Cluster
description: Working with replication and clustering for Redis. 
published: true
date: 2022-12-19T18:29:03.437Z
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
replica-announce-ip redis-0
```

```
# Worker nodes
protected-mode no
port 6379
slaveof redis-0 6379
masterauth password
requirepass password
replica-announce-ip redis-1
```

The *masterauth* password is used by the workers to connect to the master node. This is also included on the master node itself becuase when running in HA, the master node can rotate among all nodes in the cluster. 

### Persistence

Set up configurations to include both modes of persistence, `RDB` & `AOF`.

#### Configuration

```
# RDB MODE
echo 'dbfilename "dump.rdb"' >> redis-{0,1,2}/redis.conf

# AOF
cat <<EOF >> redis-{0,1,2}/redis.conf
dir /data
appendonly yes
appendfilename "appendonly.aof"
EOF
```

### Start Containers

If running on the same local machine, make sure you have a docker network called `redis` created.

```
docker run -d --rm --name redis-0 \
  --net redis \ 
  -v ${PWD}/redis-0:/etc/redis/ \
  redis:6.2-alpine redis-server /etc/redis/redis.conf
```

#### Single Liners (same command above):
```
docker run -d --rm --name redis-0 --net redis -v ${PWD}/redis-0:/etc/redis/ redis:6.2-alpine redis-server /etc/redis/redis.conf
```

```
docker run -d --rm --name redis-1 --net redis -v ${PWD}/redis-1:/etc/redis/ redis:6.2-alpine redis-server /etc/redis/redis.conf
```

```
docker run -d --rm --name redis-2 --net redis -v ${PWD}/redis-2:/etc/redis/ redis:6.2-alpine redis-server /etc/redis/redis.conf
```

---

Next, run a [simple application](https://github.com/andygodish/redis-docker/tree/main/applications/client) that can connect to the master node and write data. 

Once data has been written to the master, exec into one of the two worker nodes and utilize the `redis-cli` to verify that the data has been replicated over to that node. 

```
docker exec -it redis-2 sh
```

```
redis-cli
```

```
auth password
```

```
keys *
```

Upon executing the previous command, you should see a key assosiated with the sample application called *counter*.

Replication does not equal HA. For that, you need to utilize Redis Sentinel

## High Availability

The purpose of Sentinel is to elect a new leader should the master node die.

You should run 3 instances of the sentinel (in additional to your redis nodes). For failover to work, there needs to be a majority vote by the sentinels. 

### Configuration

- Tell sentinel which node to monitor (aka your master) and give it a name (mymaster)
- if the master is down for more than 5000 milliseconds, start the failover process

**NOTE**: I ran into an error where sentinel wasn't able to connect to the redis-0 hostname (docker container name). I was able to resolve with this stackoverflow answer: 

> Only sentinel with version above 6.2 can resolve host names, but this is not enabled by default. Adding sentinel resolve-hostnames yes to sentinel.conf will help.
>
> If your sentinel has older versions, the hostname redis_node should be replaced by and ip.
> 
> For more details, check out IP Addresses and DNS names in Redis document

```
mkdir sentinel-{0,1,2}

cat <<EOF > sentinel-{0,1,2}/sentinel.conf
port 5000
sentinel resolve-hostnames yes
sentinel monitor mymaster redis-0 6379 2
sentinel down-after-milliseconds mymaster 60000
sentinel failover-timeout mymaster 180000
sentinel parallel-syncs mymaster 1
EOF

echo "sentinel announce-ip redis-0" >> sentinel-0/sentinel.conf
echo "sentinel announce-port 5000" >> sentinel-0/sentinel.conf
echo "sentinel announce-ip redis-1" >> sentinel-1/sentinel.conf
echo "sentinel announce-port 5000" >> sentinel-1/sentinel.conf
echo "sentinel announce-ip redis-2" >> sentinel-2/sentinel.conf
echo "sentinel announce-port 5000" >> sentinel-2/sentinel.conf
```

Running the sentinel containers in docker:

- Note that this is using the same redis image
	- You start in *sentinel* mode by running `redis-sentinel`
```
docker run -d --rm --name sentinel-0 --net redis \
    -v ${PWD}/sentinel-0:/etc/redis/ \
    redis:6.0-alpine \
    redis-sentinel /etc/redis/sentinel.conf
```
#### Single Liners (same command above):

```
docker run -d --rm --name sentinel-0 --net redis -v ${PWD}/sentinel-0:/etc/redis/ redis:6.2-alpine redis-sentinel /etc/redis/sentinel.conf
```

```
docker run -d --rm --name sentinel-1 --net redis -v ${PWD}/sentinel-1:/etc/redis/ redis:6.2-alpine redis-sentinel /etc/redis/sentinel.conf
```

```
docker run -d --rm --name sentinel-2 --net redis -v ${PWD}/sentinel-2:/etc/redis/ redis:6.2-alpine redis-sentinel /etc/redis/sentinel.conf
```

### Check the Health of the Sentinels

```
docker logs sentinel-0
```

Look for information indicating the *+monitor* is up and running as well as both of your relipicas as indicated by a *+fix-slave-config* in the logs. You'll also want to see information about your other two sentinel servers. 

#### Exec into Sentinel Containers

```
docker exec -it sentinel-0 sh
redis-cli -p 5000
info
sentinel master mymaster
```

Verify by looking at the number of sentinels line in the output of the last command. That should read 2.

