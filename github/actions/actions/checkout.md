---
title: Actions/Checkout
description: Using the publicly checkout action by "actions"
published: true
date: 2023-06-28T04:13:46.101Z
tags: cicd, github actions, checkout
editor: markdown
dateCreated: 2023-06-28T04:02:41.945Z
---

# Actions/Checkout

- [Actions Marketplace](https://github.com/marketplace/actions/checkout)

#### Example:

```
- name: Checkout
  uses: actions/checkout@v3
  with:
    fetch-depth: 0
```

When you have a version of @v3, you will automatically pull in the latest 3.y.z release available. You can view that in the release section of the action's github (https://github.com/actions/checkout/releases). 

I've seen the version referenced using the commit SHA like below. I'm not exactly sure why this is used, but I suspect that it has to do with deployment in an air gapped environment:

```
- name: Checkout
  uses: actions/checkout@8e5e7e5ab8b370d6c329ec480221332ada57f0ab # v3.5.2
  with:
    fetch-depth: 0
```

You can also derive the marketplace link from the `uses` field as well, https://github.com/marketplace/actions/checkout. You can link directly to a specific version by using the `version` query parameter: `?version=v3.5.2`. You cannot link directly to the marketplace for your action using the commit SHA.  

## Fetch Depth

The default is `fetch-depth: 1`, which only pulls the latest commit. A fetch-depth of 0 will pull in the entire history which can be useful in cases where your workflow needs access to all of the repository's history, such as when you're generating a changelog based on commit messages, or when you're deploying your code and want to include a version number based on the number of commits.
