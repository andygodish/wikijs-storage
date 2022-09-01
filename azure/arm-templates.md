---
title: ARM Templates
description: Overview of ARM Templates
published: true
date: 2022-09-01T02:41:08.713Z
tags: azure
editor: markdown
dateCreated: 2022-09-01T02:41:08.713Z
---

# ARM Templates	

ARM templates are idempotent manifests representing Azure resources. They are definied in json files and can include variable injections. For example:

```
# .json file

"name": "{yourNameVariable}"
```

These are similar to cloud formation templates in AWS. 