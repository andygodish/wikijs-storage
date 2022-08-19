---
title: Ansible in Docker
description: A wiki page dedicated to exploring various ways to package container images used to execute playbooks.
published: true
date: 2022-08-18T04:09:29.960Z
tags: ansible, docker
editor: markdown
dateCreated: 2022-08-17T03:12:17.961Z
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

### Noninteractive Frontend

A bit on the `DEVIAN_FRONTNED` variable set in the subsequent commands: 

[Referece](https://askubuntu.com/questions/972516/debian-frontend-environment-variable)

Simply prepending an apt command with `DEBIAN_FRONTEND=something` does not persist after the single command to which it is applied.

`noninteractive` - This is the anti-frontend. It never interacts with you  at  all, and  makes  the  default  answers  be used for all questions. It might mail error messages to root, but that's it;  otherwise  it is  completely  silent  and  unobtrusive, a perfect frontend for automatic installs. If you are using this front-end, and require non-default  answers  to questions, you will need to preseed the debconf database; see the section below  on  Unattended  Package Installation for more details.

### sshpass

[Article explaining this tool](https://www.redhat.com/sysadmin/ssh-automation-sshpass)



