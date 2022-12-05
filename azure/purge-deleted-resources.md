---
title: Purge Deleted Resources
description: How to purge resources that have already been deleted.
published: true
date: 2022-12-05T19:22:03.716Z
tags: azure, terraform, azure-cli
editor: markdown
dateCreated: 2022-12-05T19:22:03.716Z
---

# Purge Deleted Resources

I ran into an issue when trying to re-run a terraform script which included resources that had been previously deleted from Azure (ie, resources with the same name previously existed).

Terraform produced an error indicating that resources needed to be purged from a queue containing deleted resources. This queue appears to be used to recover deleted resources. 

## Using the CLI to Purge Resources

### Cognitive Resources Example

First, list the deleted resources:

```
az cognitiveservices account list-deleted
```

In the output, use the IDs to gather the information needed in the subsequent command:

```
"id": "/subscriptions/<sub>/providers/Microsoft.CognitiveServices/locations/<location>/resourceGroups/<resource-group>/deletedAccounts/<resource-name>"
```

Use the location, resource-name, and resource-group in the following command to purge the resource:

```
az cognitiveservices account purge -l <location> -n <resource-name> -g <regourse-group>
```