---
title: Ansible in Docker
description: A wiki page dedicated to exploring various ways to package container images used to execute playbooks.
published: true
date: 2022-07-23T18:49:14.761Z
tags: ansible, docker
editor: markdown
dateCreated: 2022-07-23T18:49:14.761Z
---

# Ansible in Docker

Exploring different ways to run ansible playbooks from a docker container. 

## Python Base Image

[Reference Article](https://iceburn.medium.com/run-ansible-with-docker-9eb27d75285b)

Docker Image: 

```
FROM python:python:3.10.5

RUN pip install pip --upgrade
RUN pip install ansible

RUN apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    sshpass

WORKDIR /work
```

Pretty straight forward. Using pip to install ansible.

A bit on the DEVIAN_FRONTNED variable set in the subsequent commands: 






