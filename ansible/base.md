---
title: Base Ansible Configuration
description: Readme for my base Ansible configuration repo. 
published: true
date: 2023-01-06T18:01:29.024Z
tags: ansible
editor: markdown
dateCreated: 2023-01-06T18:01:29.024Z
---

# Ansible Base

This repo is used as a scaffolding for new Ansible projects. 

## Configuration

Everything stems from the `ansible.cfg` configuration file. A little on some of the settings it configures: 

1. inventory

- This defines a local inventory file that playbooks should utilize by default
- Points to ./inventory

2. roles_path

- This is the directory containing the roles used by your Ansible Project
- points to ./roles

## Dockerfile

Rather than running and maintaining Ansible on my local machine, I like spin up a Docker container with `sshpass` to allow ansible to execute remote commands on the target inventory. 

- You can probably build on this by referencing [this repo](https://github.com/willhallonline/docker-ansible)
- Largely based on [this guide](https://iceburn.medium.com/run-ansible-with-docker-9eb27d75285b)

### Build Local Image

```
docker build -t ansible-base:local .
```

### Test Adhoc Command

For testing purposes, this adhoc command can be used to troubleshoot the verbose output. 

```
sh ping-adhoc.sh
```

There's also a test playbook that accesses the ping role that can be executed with the following command:

```
docker run \
-v ${PWD}:/work:ro \
-v ${PWD}/roles:/root/.ansible/roles \
-v /tmp/.test-ssh:/root/.ssh \
--rm ansible-base:local ansible-playbook ping-playbook.yaml
```
