---
title: Docker Volume Backups
description: Backing up a docker volume.
published: true
date: 2024-01-27T18:29:50.000Z
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

> When deploying with docker compose, the volume name will be prefixed with the name of the root directory of the docker-compose.yaml file. For example, if your docker-compose file is located in a directory called `"wordpress-dev"`, docker will create a volume called `"wordpress-dev_db_data"` corresponding to the snippets of code above.
