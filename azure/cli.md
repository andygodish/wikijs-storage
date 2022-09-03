---
title: Azure CLI 
description: Overview of the Azure CLI tool.
published: true
date: 2022-09-03T20:28:29.336Z
tags: azure
editor: markdown
dateCreated: 2022-09-03T20:15:28.473Z
---

# Azure CLI 

- [Installation Docs](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
- `v2.39.0` at the time of this writing

Quick install command. I trust Microsoft: 

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

## Upgrade with CLI 

```
az upgrade
```

## Login via CLI

This command will open a web browser for you to continue your login process. You'll see all the subscriptions assigned to the logged in account ouputted once complete. 

```
az login
```

Confirm current supscription for you account:

```
az account show
```

- Query: view all subscription names and ids:

These queries are case sensitive.

```
az account list --query "[?user.name=='Andrew.P.Godish@vailresorts.com'].{Name:name, ID:id, Default:isDefault}" --output Table
```



