---
title: Create a Proxmox VM Template with Packer
description: Create a Proxmox VM Template with Packer
published: true
date: 2024-01-04T22:21:42.497Z
tags: proxmox, packer, iac
editor: markdown
dateCreated: 2024-01-04T21:29:35.148Z
---

# Create a Proxmox VM Template with Packer

- [Github Repo](https://github.com/andygodish/IaC/tree/main/hashicorp/packer/proxmox)
- [Wikijs Storage](https://github.com/andygodish/wikijs-storage/blob/main/proxmox/packer-vm-template.md)
---
- [Reference Repository(dustinrue)](https://github.com/dustinrue/proxmox-packer)
- [Reference Repository(ChristianLempa)](https://github.com/ChristianLempa/boilerplates/tree/main/packer/proxmox/ubuntu-server-focal)
- [YouTube Video](https://www.youtube.com/watch?v=1nf3WOEFq1Y)
---

The IaC repo linked above contains a blueprint for creating Packer manifests for VM templates in Proxmox. 

## Quickstart

This assumes the targeted `.iso` file has been uploaded to the configured location in Proxmox, a connection from the docker host can be made to your proxmox server, and an API token with adequate permissions has been created on your target Proxmox node. 

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

We are mounting the codebase of the proxmox directory containing our template manifests into the working directory of the container (/app).

### --network host

This one hung me up for a while. I was originally attempting to execute the ubuntu2204 manifest from a WSL instance running on my Windows laptop. Part of the packer process involves the creation of an HTTP server that hosts and serves the cloud-init configuration files. The Proxmox server needs to be able to connect to the host IP running the docker container, not the IP of the container itself. 

For example,

#### Ubuntu2204

packer.pkr.hcl contains a `boot_command` that references the Packer HTTP server:
```
"linux /casper/vmlinuz -- autoinstall ds='nocloud-net;s=http://{{ .HTTPIP }}:{{ .HTTPPort }}/'"
```
The `{{ .HTTPIP }}` is derived from the container instance. If `--network host` is not defined in the run command, the contianer IP will be used instead of the host IP and traffic from the Proxmox server will be unable to reach the container. 

6. Create a Proxmox VM Template

From inside the container instance run `make ubuntu2204`.

This should kick off the creation process. Monitor the progress from the command line and the Proxmox console. 

In Proxmox, you'll observe the creation of a tempory VM, named like so: 

![packer-proxmox-initial-vm.png](/images/packer-proxmox-initial-vm.png)

This will boot a new instance based on the `.iso` provided to Packer and configure it based on the inputs provided in the manifest. Once the initial boot is completed, the VM will be converted to a template by Packer. 

