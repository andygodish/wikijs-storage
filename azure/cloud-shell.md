---
title: Cloud Shell
description: Overview of the cloud shell feature in Azure.
published: true
date: 2022-08-31T22:48:49.502Z
tags: azure, cloud-shell
editor: markdown
dateCreated: 2022-08-31T22:35:15.516Z
---

# Cloud Shell

Cloud Shell is an interactive bash or powershell cli accessable from the Azure web portal. It comes packaged with the `az` cli tool configured to work with your current account. 

In order to use cloud shell, you need to specify a storage account and file share service. These are used to persist your file system between sessions. 

When opening your account's cloud shell for the first time, you can create your storage account and file share resources from the cloud shell terminal by clicking `advanced settings`.

## Create a VM

```
az vm create \
> --name LabVM \
> --resource-group 72-ab4623df-accessing-and-using-the-azure-cloud-sh \
> --image UbuntuLTS \
> --admin-username azureuser \
> --generate-ssh-keys
```

## Powershell Cmdlets

Some examples, look at the documentation for a complete list. Not sure how useful this will be given my prefrence for bash. 

```
Get-AzResourceGroup
Get-AzStorageAccount
```