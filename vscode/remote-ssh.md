---
title: Remote-SSH Extension
description: Basics on how to use the Remote-SSH Extension in vscode.
published: true
date: 2024-01-19T16:35:41.598Z
tags: ssh, dev, vscode
editor: markdown
dateCreated: 2024-01-19T16:35:41.598Z
---

# Remote-SSH Extension

My use case involves a wordpress development environment on a remote server running in my homelab. The server hosts docker containers that comprise my development environment. Updates to the codebase can be made on the remote server directly using this exension.

## Installation 

Search for `Remote SSH`. It is related to the WSL extension I use for development on my Windows machine. 

![remote-ssh.png](/images/remote-ssh.png)

## Configuration

Follow this [guide](https://code.visualstudio.com/docs/remote/remote-overview#_getting-started) for complete details. 

Open vscode, hit `F1` to open your configuratio menu, and search for `Remote-SSH: Open SSH Configuration File...`. Select the configuration file used by your User account and add your remote server configuration: 

```
Host wp-dev
  HostName 192.168.1.236
  User ubuntu
  IdentityFile C:\Users\your-user\.ssh\id_rsa
  ServerAliveInterval 60
  ServerAliveCountMax 10
```

**NOTE:** When using WSL on your Windows machine, you're going to want to reference your ssh `config` file in your Windows filesystem. 

## Connect

`F1` --> `Remote-SSH: Connect to Host...` --> Select your newly configured host. 

This will open a new vscode window and prompt you to answer some basic questions about your server, kicking off the installation of vscode on the remote server. From this point on, you can run vscode remotely over ssh from your local machine. 


