---
title: Azure Backend for Remote State
description: Using an Azure storage account for terraform remote state. 
published: true
date: 2023-05-03T14:46:40.590Z
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

Default is hot, but cool may be an adequate solution for additional cost management. 