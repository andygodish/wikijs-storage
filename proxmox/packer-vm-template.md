---
title: Create a Proxmox VM Template with Packer
description: Create a Proxmox VM Template with Packer
published: true
date: 2024-01-04T21:55:41.247Z
tags: proxmox, packer, iac
editor: markdown
dateCreated: 2024-01-04T21:29:35.148Z
---

# Create a Proxmox VM Template with Packer

- [Github Repo](https://github.com/andygodish/IaC/tree/main/hashicorp/packer/proxmox)
- [Wikijs Storage](https://github.com/andygodish/wikijs-storage/blob/main/proxmox/packer-vm-template.md)
---

The IaC repo linked above contains a blueprint for creating Packer manifests for VM templates in Proxmox. 

## Quickstart

1. Clone the IaC repo.
```
git clone https://github.com/andygodish/IaC.git
```
2. Navigate to the packer directory.
```
cd Iac/hasicorp/packer
```
- This directory contains a docker image that allows you to execute packer commands from a docker container. 

3. Build the docker image.
```
docker build -t packer:local -f 1.10.dockerfile .
```
- this dockerfile also install `make` to allow for the execution of commands found in this [Makefile](https://github.com/andygodish/IaC/blob/main/hashicorp/packer/proxmox/Makefile).
- It also sets the working directory to `/app`

4. Set Packer environment variables. 

The Makefile used to execute the Packer template builds looks for environment variables set in the  `variables.pkrvars.hcl` file. The \*-example.hcl file contains the required variables. Copy it to the properly named file and set the values according to your environment. Make sure to keep sensitive information safe. 

```
cp variables.pkrvars-example.hcl variables.pkrvars.hcl
```

5. Run the local container, mounting the Proxmox directory

Navigate to the Proxmox directory. 

```
cd Iac/hasicorp/packer/proxmox

docker run -it --network host --entrypoint=/bin/bash -v $PWD:/app packer:local
```

### Volume Mount

We are mounting the codebase of the proxmox directory containing our template manifests into the working directory of the 

