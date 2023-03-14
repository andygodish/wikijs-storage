---
title: Minimum Version Requirements
description: Short script for determining if your cluster meets the minimum requirements for flux. 
published: true
date: 2023-03-14T15:47:58.073Z
tags: kubernetes, bash, flux
editor: markdown
dateCreated: 2023-03-14T15:47:58.073Z
---

# Minimum Requirements

- [Docs] (https://fluxcd.io/flux/installation/#prerequisites)

```
echo "Installing flux from kustomization"
KUBECTL_VERSION=$(kubectl version --client --short | awk -F "v" '{print $NF}')
KUBECTL_MIN_VERSION="1.23.0"

if [ "$(printf '%s\n' "$KUBECTL_MIN_VERSION" "$KUBECTL_VERSION" | sort -V | head -n1)" = "$KUBECTL_MIN_VERSION" ]; then
  kubectl kustomize "$FLUX_KUSTOMIZATION" | sed "s/registry1.dso.mil/${REGISTRY_URL}/g" | kubectl apply -f -
else
  if [ command -v kustomize ] >/dev/null 2>&1; then
    echo "Kustomize not found"
    exit 1
  else
    kustomize build "$FLUX_KUSTOMIZATION" | sed "s/registry1.dso.mil/${REGISTRY_URL}/g" | kubectl apply -f -
  fi
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