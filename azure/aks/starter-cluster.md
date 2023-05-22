---
title: Getting Started with AKS
description: Stand up a starter cluster with the Azure CLI. 
published: true
date: 2023-05-22T02:25:42.312Z
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

## Creating the Cluster

Modify the following command to your liking: 

```
az aks create -n aks-getting-started \
--resource-group $RESOURCEGROUP \
--location uswest2 \
--kubernetes-version 1.16.10 \
--load-balancer-sku standard \
--nodepool-name default \
--node-count 1 \
--node-vm-size Standard_E4s_v3  \
--node-osdisk-size 250 \
--ssh-key-value ./id_rsa.pub \         ### if blank, uses id_rsa
--network-plugin kubenet \
--service-principal $SERVICE_PRINCIPAL \
--client-secret "$SERVICE_PRINCIPAL_SECRET" \
--output none
```

## Obtain Credentials (kubeconfig)

I'm assuming the account used to sign in via the CLI (`az login`) needs to have adequate permissions inorder for this admin flag to work properly. 

Once created, it is up to the admin to set up RBAC policies for additional users. 

```
az aks get-credentials -n \<aks-name> -g $RESOURCE_GROUP --admin
```


