---
title: Azure Backend for Remote State
description: Using an Azure storage account for terraform remote state. 
published: true
date: 2023-05-03T21:43:59.092Z
tags: azure, terraform
editor: markdown
dateCreated: 2023-05-03T14:44:33.317Z
---

# Azure Backend for Remote State	

## GRS vs LRS 

### Cost Considerations

How important is data redundancy when considering terraform state?

> Locally redundant storage (LRS) copies your data synchronously three times within a single physical location in the primary region. LRS is the least expensive replication option, but isn't recommended for applications requiring high availability or durability.

I think for the purposes of the Dr Fence project (and likely all terraform remote state), LRS will be fine. 

## Access Tier

Research indicates hot as the commonly used tier for this use case. Going with that until otherwise proven to be unnecessary.

## Bash Script for Creating Remote State Resources

```
RESOURCE_GROUP_NAME=kopicloud-tstate-rg
STORAGE_ACCOUNT_NAME=kopicloudtfstate$RANDOM
CONTAINER_NAME=tfstate
# Create resource group
az group create --name $RESOURCE_GROUP_NAME --location "West Europe"
# Create storage account
az storage account create --resource-group $RESOURCE_GROUP_NAME --name $STORAGE_ACCOUNT_NAME --sku Standard_LRS --encryption-services blob
# Get storage account key
ACCOUNT_KEY=$(az storage account keys list --resource-group $RESOURCE_GROUP_NAME --account-name $STORAGE_ACCOUNT_NAME --query [0].value -o tsv)
# Create blob container
az storage container create --name $CONTAINER_NAME --account-name $STORAGE_ACCOUNT_NAME --account-key $ACCOUNT_KEY
echo "storage_account_name: $STORAGE_ACCOUNT_NAME"
echo "container_name: $CONTAINER_NAME"
echo "access_key: $ACCOUNT_KEY"
```
