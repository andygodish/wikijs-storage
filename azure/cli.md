---
title: Azure CLI 
description: Overview of the Azure CLI tool.
published: true
date: 2022-09-03T22:38:02.032Z
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

## Change Azure Subscription

```
az account set --subscription "<sub>"
```

Note that there is no output for this command, you'll need to follow up with an `az account show` command for confirmation. 

## Authenticate with a Service Principle

```
az login
```

This step is not necessary if using the cloud shell:
```
export MSYS_NO_PATHCONV=1
```

Create a service principa:

This will be needed for authentication through the CLI for use with tools such as terraform. 

```
az ad sp create-for-rbac --name andyg-svc-principal --role Contributor
```

I got dingged for not providing a scope:
- `Usage error: To create role assignments, specify both --role and --scopes.`

Then a again for not having enough permissions to use the subscriptionId as my scope:
- `Directory permission is needed for the current user to register the application. For how to configure, please refer 'https://docs.microsoft.com/azure/azure-resource-manager/resource-group-create-service-principal-portal'. Original error: Insufficient privileges to complete the operation.`
```
az ad sp create-for-rbac --name andyg-svc-principal --role Contributor --scope a4dc843c-36db-48d2-81e8-90f658f2dc67
```






