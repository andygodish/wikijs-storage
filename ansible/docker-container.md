---
title: Ansible in Docker
description: A wiki page dedicated to exploring various ways to package container images used to execute playbooks.
published: true
date: 2024-01-05T16:43:43.910Z
tags: ansible, docker
editor: markdown
dateCreated: 2023-10-08T22:19:26.587Z
---

# Ansible

- [Github Repo](https://github.com/andygodish/IaC)
- [Wikijs Documentation](https://github.com/andygodish/wikijs-storage/blob/main/ansible/docker-container.md)

---

Run Ansible from a docker container.

## Build the Ansible Container Images

Rather than running and maintaining Ansible on my local machine, I like spin up a Docker container with `sshpass` to allow ansible to execute remote commands on the target inventory.

```
cd ansible

docker build -t ansible:local .
```

The dockerfile in this directory contains instructions for installing Ansible ontop of a python base image along with the ssh dependencies required to connect to a target machine via ssh. 

- [sshpass article](https://www.redhat.com/sysadmin/ssh-automation-sshpass)
- this project is based largely on [this repo](https://github.com/willhallonline/docker-ansible)
- And on [this guide](https://iceburn.medium.com/run-ansible-with-docker-9eb27d75285b)

The subsequent directory tree is broken up into playbooks/roles for specific tasks (playbook directories). In most cases, ansible-galaxy is used as an entrypoint command to install or upgrade particular roles. Those roles are then edited to fit the needs of the project. 

## Configuration

Everything stems from the `ansible.cfg` configuration file. Some of the settings it configures:

1. inventory

- This defines a local inventory file that playbooks should utilize by default
- Points to the `./inventory` directory within the project directory

#### hosts.ini

This file defines the target host IPs and the ssh user/private key used by ansible at execution time. The defined `ansible_ssh_private_key_file` is the location of the ssh private key needed for the container to ssh into your target host. 

2. roles_path

- This is the directory containing the roles used by your Ansible Project
- points to `./roles`

## Executing a Playbook

Mount the ansible scripts and configuration files within each playbook directory in your docker run command: 

> Make sure your volume mounts are referring to the correct directory

```
docker run \
-v ${PWD}:/app \
-v ${PWD}/roles:/app/.ansible/roles \
-v ~/.ssh:/app/.ssh \
--rm ansible:local ansible-playbook main.yaml
```

`-v ~/.ssh:/app/.ssh \` --- this volume mount corresponds to the `ansible_ssh_private_key_file` field definted in each playbook directory's `hosts.ini` file. 






