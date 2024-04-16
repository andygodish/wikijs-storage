---
title: Create a Proxmox VM Template with Packer
description: Create a Proxmox VM Template with Packer
published: true
date: 2024-04-16T15:05:54.532Z
tags: proxmox, packer, iac
editor: markdown
dateCreated: 2024-01-04T21:29:35.148Z
---

# Create a Proxmox VM Template with Packer

- [Github Repo](https://github.com/andygodish/IaC/tree/main/hashicorp/packer/proxmox)
- [Wikijs Documentation](https://github.com/andygodish/wikijs-storage/blob/main/proxmox/packer-vm-template.md)
---
- [Packer Documentation for Proxmox](https://developer.hashicorp.com/packer/integrations/hashicorp/proxmox/latest/components/builder/iso#network-adapters)
- [Reference Repository(dustinrue)](https://github.com/dustinrue/proxmox-packer)
- [Reference Repository(ChristianLempa)](https://github.com/ChristianLempa/boilerplates/tree/main/packer/proxmox/ubuntu-server-focal)
- [YouTube Video](https://www.youtube.com/watch?v=1nf3WOEFq1Y)

## Quickstart

This assumes the targeted `.iso` file has been uploaded to the configured location in Proxmox, a connection from the docker host can be made to your proxmox server, and [an API token with adequate permissions has been created](https://github.com/andygodish/wikijs-storage/blob/main/proxmox/create-api-key.md) on your target Proxmox node. 

1. Clone the  repo.
```
git clone https://github.com/andygodish/packer-docker
```
The root directory contains a docker image that allows you to execute packer commands from inside a container. 

3. Build the docker image.
```
docker build -t packer -f 1.10.dockerfile .
```

4. Set Packer environment variables. 

```
cd proxmox
```

The Makefile used to execute the Packer template builds looks for environment variables set in the  `variables.pkrvars.hcl` file. The \*-example.hcl file contains the required variables. Rename it to the properly named file and set the values according to your environment. Make sure to keep sensitive information safe. 

```
cp variables.pkrvars-example.hcl variables.pkrvars.hcl
```

5. Run the local container, mounting the Proxmox directory

Navigate to the Proxmox directory. 

```
cd proxmox
```
```
docker run -it --network host --entrypoint=/bin/bash -v $PWD:/app packer
```

### Volume Mount

We are mounting the codebase of the proxmox directory containing our template manifests into the working directory of the container (/app).

### --network host

This one hung me up for a while. I was originally attempting to execute the ubuntu2204 manifest from a WSL instance running on my Windows laptop. Part of the packer process involves the creation of an HTTP server that hosts and serves the cloud-init configuration files. The Proxmox server needs to be able to connect to the host IP running the docker container, not the IP of the container itself. 

> The `--network host` tag is required, but this process does not work using WSL. Even though my Windows laptop is on the same network as the Proxmox host, I was unable to make the connection through my Windows IP down to the bridge to my WSL instance. I need to use another local machine on my network that does not use WSL.

This machine will need docker installed.

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

## Create an API Token

## Issues

### Keyboard Layout

Regarding this piece from the `cloud-config` file (ubuntu2204):

```
autoinstall:
  locale: en_US
  keyboard:
    layout: us
    # variant: en --- this is not needed
```

I originally had `keyboard.varient` set to `en`. The boot process would crash due to an invalid field. This value appears to have been valid in the past. 

### Boot Command

#### Ubuntu2204

It took me a while to find a valid set of boot commands:

```
  boot_command = [
    "c",
    "<wait5>",
    "linux /casper/vmlinuz -- autoinstall ds='nocloud-net;s=http://{{ .HTTPIP }}:{{ .HTTPPort }}/'",
    "<enter><wait><wait>",
    "initrd /casper/initrd",
    "<enter><wait><wait>",
    "boot<enter>"
  ]
```

It appears that different commands are required for different versions of ubuntu. 
