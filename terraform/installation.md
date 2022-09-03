---
title: Terraform Installation and Configuration 
description: Installation and configuration guide for terraform. 
published: true
date: 2022-09-03T18:43:51.351Z
tags: terraform
editor: markdown
dateCreated: 2022-09-03T18:39:17.419Z
---

# Terraform Installation and Configuration 

- [Installation Docs (Ubuntu)](https://learn.hashicorp.com/tutorials/terraform/install-cli)
- `v1.2.8` at the time of this writing

Update system and install prereqs:

```
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```

Hashicorp gpg key:

```
wget -O- https://apt.releases.hashicorp.com/gpg | \
    gpg --dearmor | \
    sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg
```

Verify the key's fingerprint

```
gpg --no-default-keyring \
    --keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    --fingerprint
```

Add Hashicorp repo to your system:

```
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
    https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/hashicorp.list
```

Update again and install

```
sudo apt update

sudo apt install terraform
```

## Enable Auto-Completion

Run this command and see the added lines at the bottom of your `~/.zshrc` file:

```
terraform -install-autocomplete
```