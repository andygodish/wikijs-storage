---
title: Upgrading Pihole
description: Process guide for upgrading Pihole in my homelab.
published: true
date: 2022-12-31T20:53:07.587Z
tags: docker, pihole, homelab
editor: markdown
dateCreated: 2022-12-31T20:53:07.587Z
---

# Upgrading Pihole

I'm running pihole via docker-compose based on the configurations set in [this repo](https://github.com/andygodish/pihole). Upgrades are pretty straight forward: 

1. Bring the current running instance down, `docker compose down`
2. Edit the docker-compose.yaml file to reflect your target version
3. Bring the container back up, `docker compose up`

## Gravity Sync

Upgrades do not appear to interrupt the gravity sync configuration between my primary and backup pihole servers. I will continue to monitor this during future upgrades. 