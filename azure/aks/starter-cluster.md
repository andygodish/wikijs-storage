---
title: Getting Started with AKS
description: Stand up a starter cluster with the Azure CLI. 
published: true
date: 2023-05-22T02:20:22.419Z
tags: kubernetes, azure, azure-cli, aks
editor: markdown
dateCreated: 2023-05-22T02:08:40.594Z
---

# Getting Started with AKS

This is a quick walkthrough of getting a small AKS cluster up in running using the Azure CLI. 

## Resource Group

```
RESOURCEGROUP=aks-getting-started
az group create -n $RESOURCEGROUP -l australiaeast
```

## Service Principle & Role Assignment

A service account needs to be created and given contributor access to the cluster's resource group. 

## Create the Service Principal

From the output of the creation command, grab the SPNs id and password.

```
SERVICE_PRINCIPAL_JSON=$(az ad sp create-for-rbac --skip-assignment --name aks-getting-started-sp -o json)

SERVICE_PRINCIPAL=$(echo $SERVICE_PRINCIPAL_JSON | jq -r '.appId')
SERVICE_PRINCIPAL_SECRET=$(echo $SERVICE_PRINCIPAL_JSON | jq -r '.password')
```

Assign the `Contributing` role to the SPN with the following attributes:
- assignee: SPN
- scope: resource group
- role: `Contributor` (Azure default)

```
az role assignment create --assignee $SERVICE_PRINCIPAL \
--scope "/subscriptions/$SUBSCRIPTION/resourceGroups/$RESOURCEGROUP" \
--role Contributor
```



