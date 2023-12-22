---
title: Azure Authentication
description: Configuring an App Registration to connect to an Azure subscription/tenant
published: true
date: 2023-12-22T05:26:21.198Z
tags: azure, terraform, authentication
editor: markdown
dateCreated: 2023-12-22T05:18:25.414Z
---

# Azure Authentication 

- [Reference Guide](https://dev.to/this-is-learning/deploy-azure-infrastructure-using-terraform-cloud-3j9d)
- [Azure Env Variables](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret.html#configuring-the-service-principal-in-terraform)
- [Terraform Cloud Azure Provider Config Docs](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials/azure-configuration)

## Azure Configuration via SPN

The following notes outline the processes described in the `Dynamic Provider Credentials` section of the Terraform Cloud documentation. 

Prerequisites to the following processes involve identifying a target subscription. 

### Register An App

This also creates a SPN that is used later to provide permissions within your target subscription. 

Navigate to `Entra ID --> App registrations` and click `New registration` in the top menu. Give it a name, check an appropriate account type, and select `web` in the platform dropdown under the Redirect URI section. Leave the URL field blank. 

Make sure you go to your new app registration and set an appropriate owner. Failing to do this will result in you being unable to assign IAM roles to the app's associated SPN within your target subscription. 

## Configure App to Trust a Generic User

Navigate to your `App registration --> Certificates & secrets --> Federated credentials (top menu)` and click `Add credential`.

[Fill it out accordingly](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials/azure-configuration#configure-azure-active-directory-application-to-trust-a-generic-issuer):

> Federated credential scenario: Must be set to Other issuer.

> Issuer: The address of Terraform Cloud (e.g., https://app.terraform.io).
Important: make sure this value starts with https:// and does not have a trailing slash.

> Subject identifier: The subject identifier from Terraform Cloud that this credential will match. This will be in the form organization:my-org-name:project:my-project-name:workspace:my-workspace-name:run_phase:plan where the run_phase can be one of plan or apply.

>Name: A name for the federated credential, such as tfc-plan-credential. Note that this cannot be changed later.



