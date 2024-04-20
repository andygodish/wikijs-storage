---
title: Ansible in Docker
description: A wiki page dedicated to exploring various ways to package container images used to execute playbooks.
published: true
date: 2024-04-08T04:15:27.422Z
tags: ansible, docker
editor: markdown
dateCreated: 2023-10-08T22:19:26.587Z
---

# Ansible in Docker

- [Github Repo](https://github.com/andygodish/ansible-docker)
- [Wikijs Documentation](https://github.com/andygodish/wikijs-storage/blob/main/ansible/docker-container.md)

---

Run Ansible from a docker container.

## Build the Ansible Container Images

Rather than running and maintaining Ansible on my local machine, I like spin up a Docker container with `sshpass` to allow ansible to execute remote commands on the target inventory.

```
git clone https://github.com/andygodish/ansible-docker.git

docker build -t ansible .
```

The dockerfile in this directory contains instructions for installing Ansible ontop of a python base image along with the ssh dependencies required to connect to a target machine via ssh. 

- [sshpass article](https://www.redhat.com/sysadmin/ssh-automation-sshpass)
- this project is based largely on [this repo](https://github.com/willhallonline/docker-ansible)
- And on [this guide](https://iceburn.medium.com/run-ansible-with-docker-9eb27d75285b)

## Bootstrapping a New Playbook

In the root directory is a script containing basic instructions for bootstrapping a new 'playbook' in its own directory. The script is baked into the dockerfile used to build the image. 

The following stands up a consistent ansible playbook directory where you can then build your own roles/tasks or pull down roles/collections from Ansible Galazy. 

```
docker run \                   
-v ${PWD}:/app \
-e ANSIBLE_PLAYBOOK_NAME=update --rm ansible /bin/bash /scripts/bootstrap-new-playbook.sh
```

### ZSH Alias

I use an alias for the playbook bootstrapping process so I can execute the script by running: `ansible-docker new playbook <playbook-name>`.

To replicate, open your `zshrc` file and add a function for a `docker-ansible` alias:

```
vim ~/.zshrc
```

```
ansible-docker() {
    if [ "$1" = "new" ] && [ "$2" = "playbook" ] && [ -n "$3" ]; then
        docker run -v ${PWD}:/app \
        -e ANSIBLE_PLAYBOOK_NAME=$3 --rm ansible /bin/bash /scripts/bootstrap-new-playbook.sh
    else
        echo "Usage: ansible-docker new role <name>"
    fi
}
```

```
source ~/.zshrc
```

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

---

The `boostrap-new-playbook.sh` script copies the hosts.ini.example file from the root dir of this repo and places it in your new playbook directory as a hosts.ini file that maps to the default value of the ansible.cfg file (also in the root dir of this repo).

So, before building the dockerfile, make sure to define your inventory. 

## Executing a Playbook

Mount the ansible scripts and configuration files within each playbook directory in your docker run command: 

> Make sure your volume mounts are referring to the correct directory

```
docker run \
-v ${PWD}:/app \
-v ~/.ssh:/home/appuser/.ssh/ \
--rm ansible ansible-playbook main.yaml
```

`-v ~/.ssh:/app/.ssh \` --- this volume mount corresponds to the `ansible_ssh_private_key_file` field definted in each playbook directory's `hosts.ini` file. 

## Ansible Galaxy

### Installing a Role or Collection

Once you have the local ansible container built, you can run `ansible-galaxy` commands just as you would execute a playbook.

Change into your directory and make sure to appropriately map a volume so the role or collection file are appropriately copied to your directory.

#### [Nodejs Role] (example) (https://galaxy.ansible.com/ui/standalone/roles/geerlingguy/nodejs/)

```
docker run -it \
-v ${PWD}:/app \
--rm ansible ansible-galaxy role install geerlingguy.nodejs
```


