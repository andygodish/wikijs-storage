---
title: Migrating Terraform State
description: Outlining the migration of Terraform state files. 
published: true
date: 2024-06-07T03:15:53.864Z
tags: terraform, terraform-cloud
editor: markdown
dateCreated: 2024-06-06T21:51:49.628Z
---

# Migrating Terraform State

## CLI Managed Remote State to Cloud

I have an existing Terraform "project" connected to remote Azure storeage account defined in a backend block for remote state management,

```
terraform {
  backend "azurerm" {
    tenant_id            = "<tenant_id>"
    subscription_id      = "<subscription_id>"
    resource_group_name  = "<resource_group_name>"
    storage_account_name = "<storage_account_name>"
    container_name       = "<container_name>"
    key                  = "<keyvault_key>"
  }
```

The goal is to migrate the state files in the storage account to Terraform Cloud for management of the project and associated workspaces.

The project was initially built on a local machine using the Terraform CLI. Two workspaces were established to allow for deployment of resources to two separate Azure subscriptions,

```
terraform-appi-splunk git:(main) terraform workspace list
  default
* dev
  prod
```




