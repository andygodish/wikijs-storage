---
title: Update Admin Password
description: How to update the random admin password that is bootstrapped by pihole.
published: true
date: 2022-12-14T02:55:57.703Z
tags: docker, pihole, homelab
editor: markdown
dateCreated: 2022-12-14T02:55:57.703Z
---

# Update Admin Password

I spin up pihole in a docker container on my LAN. I don't explicitly set a password in the configuration file. Instead, a password is randomly created by pihole. To find it, run the following docker command: 

```
docker logs <pihole-container> | grep random
```

It's pretty clearly labeled. 

## Change Bootstrapped Password

There is no option to do this via the UI. Instead, exec into the running container, 

```
docker exec -it <pihole> bash
```

and run the following command: 

```
pihole -a -p
```

This will prompt you to enter a new password that can be used to log into pihole via the UI. 