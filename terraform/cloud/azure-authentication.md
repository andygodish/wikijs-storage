---
title: Azure Authentication
description: Configuring an App Registration to connect to an Azure subscription/tenant
published: true
date: 2023-12-22T05:18:25.414Z
tags: azure, terraform, authentication
editor: markdown
dateCreated: 2023-12-22T05:18:25.414Z
---

# Azure Authentication

- [Reference Guide](https://dev.to/this-is-learning/deploy-azure-infrastructure-using-terraform-cloud-3j9d)
- [Azure Env Variables](https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/service_principal_client_secret.html#configuring-the-service-principal-in-terraform)
- [Terraform Cloud Azure Provider Config Docs](https://developer.hashicorp.com/terraform/cloud-docs/workspaces/dynamic-provider-credentials/azure-configuration)


## TLDR 

- Create an SPN (App Registration) in Azure Entra ID.
	- Configure it to allow a connection from tf cloud by adding a federated credential
  - Configure your tf cloud workspace via env variables (client ID + tenant ID)
  
  

, configure it to allow a connection from your terraform cloud account in Azure via a federated credential and through the use of environment envirables from within your Terraform Cloud workspace, 

## Azure Configuration

Prerequisites to the following processes involve identifying a target subscription. 

### Register An App

This also creates a SPN that is used later to provide permissions within your target subscription. 

Navigate to `Entra ID --> App registrations` and click `New registration` in the top menu. Give it a name, check an appropriate account type, and select `web` in the platform dropdown under the Redirect URI section. Leave the URL field blank. 

Make sure you go to your new app registration and set an appropriate owner. Failing to do this will result in you being unable to assign IAM roles to the app's associated SPN within your target subscription. 


