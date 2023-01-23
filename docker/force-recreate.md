---
title: Force Recreate
description: Usage of the force-recreate flag with the docker cli.
published: true
date: 2023-01-23T22:49:46.046Z
tags: 
editor: markdown
dateCreated: 2023-01-23T22:49:46.046Z
---

# Force Recreate

## Real Life Example

I often update a configuration file used by the Traefik instance running in my homelab, for example, when I want to add additional url configurations for proxying. Once updated, I need to recreate the container in order for it to pick up the new configuration changes. To do so, I run the following command: 

```
docker-compose up -d --force-recreate
```

This will result in the previous container tearing down and a new one spun up in its place. 