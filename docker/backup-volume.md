---
title: Docker Volume Backups
description: Backing up a docker volume.
published: true
date: 2024-01-27T18:37:32.590Z
tags: docker, storage, backup
editor: markdown
dateCreated: 2024-01-27T18:29:50.000Z
---

# Docker Volume Backups

In this scenario, I have a mysql database setup for a WordPress deployment. The deployment is done via a docker-compose.yaml that maps a local docker `volume` to the `var/lib/mysql` folder of the mysql container: 

```
mysql:
  image: mysql:5.7
  volumes:
    - db_data:/var/lib/mysql 
  ...
```

The volume creation is also defined in the docker-compose.yaml:

```
volumes:
  db_data:
```

> When deploying with docker compose, the volume name will be prefixed with the name of the root directory of the docker-compose.yaml file. For example, if your docker-compose file is located in a directory called `"wordpress-dev"`, docker will create a volume called `"wordpress-dev_db_data"` corresponding to the snippets of code above. Other docker resources (ex: containers) will be prefixed with the directory name as well. 

## Identify the Volume Location

Use `docker volume list` to verify the name of your volume and `docker volume inspect wordpress-dev_db_data` to find the location of the volume files on your local docker host:

```
docker volume inspect wordpress-dev_db_data
---
[
    {
        "CreatedAt": "2024-01-05T20:37:07Z",
        "Driver": "local",
        "Labels": {
            "com.docker.compose.project": "wordpress-236",
            "com.docker.compose.version": "2.21.0",
            "com.docker.compose.volume": "db_data"
        },
        "Mountpoint": "/var/lib/docker/volumes/wordpress-236_db_data/_data",
        "Name": "wordpress-236_db_data",
        "Options": null,
        "Scope": "local"
    }
]
```

The `Mountpoint` in the above output is the location of your data on the local docker host. 

## Create a Backup

To back up this data, you can simply copy the contents of this directory to a backup location.

It's important to ensure the MySQL container is not actively writing to the database when you do this, to avoid a corrupted backup. You can stop the container temporarily:

```
docker stop 
```

