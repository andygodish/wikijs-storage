---
title: Adding Count Logic to Existing Module
description: Gotcha I found working with an existing Azure module that I needed to add 'count' logic to.
published: true
date: 2024-06-20T14:32:28.783Z
tags: azure, terraform
editor: markdown
dateCreated: 2024-06-20T14:31:50.795Z
---

# Adding Count Logic to Existing Module

## Scenario

In this scenario, I was working with a terraform project involving four separate workspaces: `dev, qa, test, & prod.`

I had initially deploy an Azure Elastic Job Agent defined as a module in my terraform code. It was later determined this resource was only going to be needed in two of the four environments, `dev & prod.` To accomplish this, I added the following "count" attribute to the module block: 

```
module "elastic_job_agent" {
  count = (terraform.workspace == "terraform-dapp-cartapi-dev" || terraform.workspace == "terraform-dapp-cartapi-prod") ? 1 : 0
  ...
}
```

## Finding

Adding this logic caused a complete tear down and recreation of the existing resource (sans count attribute). The state now references the resource at position `0` in an array instead of a standalone resource. So the resource, as defined in the `terraform state` went from this,

```
"module": "module.elastic_job_agent"
```
to this,
```
"module": "module.elastic_job_agent[0]"
```

I wasn't expecting this and it caused some manual work to have to be redone by the DBA working with the existing module. 