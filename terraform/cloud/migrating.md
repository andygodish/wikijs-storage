---
title: Migrating Terraform State
description: Outlining the migration of Terraform state files. 
published: true
date: 2024-06-11T15:24:15.721Z
tags: terraform, terraform-cloud
editor: markdown
dateCreated: 2024-06-06T21:51:49.628Z
---

# Migrating Terraform State

## CLI Managed Remote State (Azure) --> Terraform Cloud

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

To migrate, first create a target workspace of type "CLI-Driven Workflow" in Terraform Cloud. 

> While the organization defined in the cloud block must already exist, the workspace does not have to; Terraform Cloud will create it if necessary. If you use an existing workspace, it must not have any existing states.

In my first attempt at this, I did not pre-define a target workspace. Terraform Cloud created a new one inside the default project with the name provided in a `cloud` block. You'll need to comment out the existing backend block (shown above) and replace it with the following cloud block:

```
terraform {
  cloud {
    organization = "<organization>"

    workspaces {
      name = "<cloud_workspace_name>"
    }
  }
}
```

Once the backend block has been replaced with a valid cloud block, you will need to sign into Terraform Cloud and perform a `terraform init` command. To sign into Terraform Cloud via the CLI, you will need to create an API token in your Account Settings. 

![account_settings_tf_cloud.png](/images/account_settings_tf_cloud.png)

You'll also need to login to Azure CLI with an account that can fetch the existing statefile. 

```
az account login
```

**IMPORTANT**: Once you execute `terraform init` you will be warned that this will only migrate the state of the currently selected workspace. You'll have to repeat this entire process for each workspace. 

### Updating the Workflow

By default, your migrated workspace will be contifued as a "CLI-Driven Workflow." To change this to a "Version Control Workflow, navigate to the workspace settings menu and select "Version Control" in the left side menu. Navigate through the configuration menu and select the appropriate VCS/repo for your project.  




