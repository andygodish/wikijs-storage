---
title: Azure Authentication
description: Configuring an App Registration to connect to an Azure subscription/tenant
published: true
date: 2023-12-22T05:42:49.803Z
tags: azure, terraform, authentication
editor: markdown
dateCreated: 2023-12-22T05:18:25.414Z
---

# Azure Authentication

- [Reference Guide](https://dev.to/this-is-learning/deploy-azure-infrastructure-using-terraform-cloud-3j9d)
- [Azure Env Variables](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret.html#configuring-the-service-principal-in-terraform)
- [Terraform Cloud Azure Provider Config Docs](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials/azure-configuration)

## Azure Configuration via SPN

This is a minimal configuration that get you set up to connect a Terraform Cloud workspace to an Azure subscription of your choosing.

The following notes outline the processes described in the `Dynamic Provider Credentials` section of the Terraform Cloud documentation. 

Prerequisites to the following processes involve identifying a target subscription. 

### Register An App

This also creates a SPN that is used later to provide permissions within your target subscription. 

Navigate to `Entra ID --> App registrations` and click `New registration` in the top menu. Give it a name, check an appropriate account type, and select `web` in the platform dropdown under the Redirect URI section. Leave the URL field blank. 

Make sure you go to your new app registration and set an appropriate owner. Failing to do this will result in you being unable to assign IAM roles to the app's associated SPN within your target subscription. 

### IAM Role Assignment

The SPN associated with the App registration must be given an adequate IAM role within your target subscription. 

## Configure App to Trust a Generic User

Navigate to your `App registration --> Certificates & secrets --> Federated credentials (top menu)` and click `Add credential`.

[Fill it out accordingly](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials/azure-configuration#configure-azure-active-directory-application-to-trust-a-generic-issuer):

> Federated credential scenario: Must be set to Other issuer.

> Issuer: The address of Terraform Cloud (e.g., https://app.terraform.io).
Important: make sure this value starts with https:// and does not have a trailing slash.

> Subject identifier: The subject identifier from Terraform Cloud that this credential will match. This will be in the form organization:my-org-name:project:my-project-name:workspace:my-workspace-name:run_phase:plan where the run_phase can be one of plan or apply.

>Name: A name for the federated credential, such as tfc-plan-credential. Note that this cannot be changed later.

## Terraform Configuration via SPN

This process involves creating a CLI-Driven Workflow. This allows you to launch a new workspace from your local machine.

You'll need terraform CLI to complete the login process on your local machine.

### Terraform Block

```
terraform {
  required_version = ">= 1.3"
  cloud {
    organization = "org-name"
    workspaces {
      name = "new-workspace-name"
    }
  }
  required_providers {
    azurerm = {
      source = "hashicorp/azurerm"
      version = ">=3.25.0"
    }
  }
}
```

Login to terraform cloud via a `terraform login` command. You'll need an API token to complete this process. 

run your `terraform init` to create the workspace in terraform cloud. You should see it via the UI. You can edit its project location via the settings menu. 

From this point, you can either use the CLI from your local machine to plan/apply your resource modules. 

### Terraform Environment Variables

The following values will need to be set as environment variables: 

| Key     | Value |
| -------- | ------- |
| TFC_AZURE_RUN_CLIENT_ID  | Application (client) ID of Azure App    |
| February | $80     |
| March    | $420    |




