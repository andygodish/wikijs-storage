---
title: Minimum Version Requirements
description: Short script for determining if your cluster meets the minimum requirements for flux. 
published: true
date: 2023-03-14T16:01:22.195Z
tags: kubernetes, bash, flux
editor: markdown
dateCreated: 2023-03-14T15:47:58.073Z
---

# Minimum Requirements

- [Docs] (https://fluxcd.io/flux/installation/#prerequisites)

Write a bash script that first captures two variables. One indicating the current version of k8s installed locally, and one indicating a static minimum required by the flux documentation. In this case, `1.23.0`.

In order to determine if the minimum version requirements are met, sort the output of both variables in ascending order, with each on a new line, and proceed only if the minimum version is the first listed. 

```
echo "Installing flux from kustomization"
KUBECTL_VERSION=$(kubectl version --client --short | awk -F "v" '{print $NF}')
KUBECTL_MIN_VERSION="1.23.0"

if [ "$(printf '%s\n' "$KUBECTL_MIN_VERSION" "$KUBECTL_VERSION" | sort -V | head -n1)" = "$KUBECTL_MIN_VERSION" ]; then
  ...
    #install via kustomize
  ...
fi
```

- The global variables only exist for the duration of the script

## awk

- `-F "v"`: This specifies the field separator for the command. Here, we set the field separator to be the letter "v". So, each line of input is split into fields based on where the "v" character occurs, with the first field being $1, the second field $2, and so on.

- `{print $NF}`: This is the action we want to perform on each line. In this case, we want to print the last field on each line. `$NF` is an awk built-in variable that represents the last field on the current line. So, for each line of input, awk will print the last field.

For example, if you have an input like:

```
Hello world version 1.2.3
```
The awk command you provided would print 1.2.3, which is the last field on the line, separated by the "v" character.