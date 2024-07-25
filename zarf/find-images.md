---
title: Find Images
description: Using Zarf to identify upstream images associated with your component. 
published: true
date: 2024-07-25T14:22:27.806Z
tags: zarf
editor: markdown
dateCreated: 2024-07-25T14:22:27.806Z
---

# Find Images	

- [Documentation](https://docs.zarf.dev/commands/zarf_dev_find-images/)

When defining a zarf component, various types of compoents can be defined as indicated in the [zarf schema](https://github.com/zarf-dev/zarf/blob/main/zarf.schema.json): 

```
"components": {
        "items": {
          "$ref": "#/$defs/ZarfComponent"
        },
        "type": "array",
        "minItems": 1,
        "description": "List of components to deploy in this package"
      },
```

A zarf component can be a number of things, for example a helm chart:

```
components:
  - name: wordpress
    charts:
      - name: wordpress
        url: oci://registry-1.docker.io/bitnamicharts/wordpress
```

How does zarf scan the upstream `ZarfComponent` for images, and how does that process change for the various component types?

Per the documentation, this may only apply to helm charts. [Source code](https://github.com/zarf-dev/zarf/blob/main/src/pkg/packager/prepare.go#L76) for later review.
