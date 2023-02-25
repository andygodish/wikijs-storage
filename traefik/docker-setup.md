---
title: Traefik on Docker
description: Setting up standalone docker instance of Traefik in my homelab. 
published: true
date: 2023-02-25T05:56:19.704Z
tags: docker, traefik, revers-proxy
editor: markdown
dateCreated: 2023-01-02T21:37:28.534Z
---

# Traefik on Docker

- [Reference Article](https://docs.technotim.live/posts/traefik-portainer-ssl/)

In my homelab, I started with Traefik by using it as a reverse proxy that I can use to provide a TLS certification to the services running within my homelab. 

## Traefik Itself

### Infrastructure

A single proxmox node running Ubuntu 22. Ansible is used for an apt update and install docker plus a few other dependencies that are not included in the Proxmox template I currently use. 

#### Installed via Ansible: 

- docker
- apache2-utils
- vim 

#### Roles: 

- bguerel.update_reboot
- geerlingguy.docker

## Adding New Endpoint Configurations

ssh into the server hosting the Traefik docker container. Change to its data directory and open the `config.yaml`. 

Add a new router and the service it points to:

```
# router
---
landing:
      entryPoints:
        - "https"
      rule: "Host(`lan.andyg.io`)"
      middlewares:
        - default-headers
        - https-redirectscheme
      tls: {}
      service: landing
```
```
# service
---
landing:
      loadBalancer:
        servers:
          - url: "http://192.168.1.159:32726"
          - url: "http://192.168.2.80:32726"
          - url: "http://192.168.2.151:32726"
          - url: "http://192.168.2.188:32726"
```

Reset the container in order to pick up the new configuration: 

```
docker-compose up -d --force-recreate
```

Add the appropriate entry to your local dns making sure to point it to the IP of your Traefik instance. 

