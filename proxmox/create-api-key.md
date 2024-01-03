---
title: Creating an API Key
description: Creating an API key in proxmox.
published: true
date: 2024-01-03T23:01:15.218Z
tags: proxmox
editor: markdown
dateCreated: 2024-01-03T18:34:01.113Z
---

# Creating an API Key

I've used API keys in my Packer projects where I need to supply credentials for Packer to access my nodes in order to create templates. 

Navigate to,

- Datacenter
	- Permissions
  		- API Tokens
      
Click `Add` in the top menu. 

Select a user from the dropdown and provide a token ID. The table listing your tokens will refer to the token ID as the Token Name.

![proxmox-api-token.png](/images/proxmox-api-token.png)

## Privilege Separation

The image above shows `Privilege Separation` as Yes, you want to make sure to uncheck this.

Unchecking this allows your api token to have the same privileges as the selected user. 