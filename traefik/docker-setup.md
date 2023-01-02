---
title: Traefik on Docker
description: Setting up standalone docker instance of Traefik in my homelab. 
published: true
date: 2023-01-02T21:37:28.534Z
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

