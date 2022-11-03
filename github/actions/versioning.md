---
title: Versioning
description: How-to on establishing a version tag in your CICD pipelines.
published: true
date: 2022-11-03T14:35:14.027Z
tags: github, cicd
editor: markdown
dateCreated: 2022-11-03T14:35:14.027Z
---

# Versioning

- [Reference Gist](https://gist.github.com/jdolitsky/e3cb82ffad6b84a0f01e2866c1f280bd)

## Helm Chart Versioning

Below is a way to utilize grep and awk to pull out the current version from a `Chart.yaml` file. 

```
- VERSION=$(cat Chart.yaml | grep -m 1 ^version:| awk '{print $2}')
- NEW_VERSION="${VERSION%.*}.$(expr ${VERSION##*.} + 1)-rc${{CF_BUILD_TIMESTAMP}}"
- sed -i "s/$VERSION/$NEW_VERSION/g" Chart.yaml
```