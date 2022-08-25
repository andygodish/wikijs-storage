---
title: Kubectx/Kubens
description: Installation and configuration steps for kubectx/kubene
published: true
date: 2022-08-25T03:40:18.447Z
tags: kubernetes, kubectl
editor: markdown
dateCreated: 2022-08-25T03:40:18.446Z
---

# Kubectx & Kubens

## Install from Repo

Clone the repository and create a symlink to its output directory.

```
sudo git clone https://github.com/ahmetb/kubectx /opt/kubectx
sudo ln -s /opt/kubectx/kubectx /usr/local/bin/kubectx
sudo ln -s /opt/kubectx/kubens /usr/local/bin/kubens
```

## AutoCompletion

Follow these steps to configure auto complete for kubectx/kubenx:

- These will come with the repo installed in the first step's target directory, `/opt/kubectx`. The raw content of the zsh scripts are below. 

```
mkdir -p ~/.oh-my-zsh/completions
chmod -R 755 ~/.oh-my-zsh/completions
ln -s /opt/kubectx/completion/_kubectx.zsh ~/.oh-my-zsh/completions/_kubectx.zsh
ln -s /opt/kubectx/completion/_kubens.zsh ~/.oh-my-zsh/completions/_kubens.zsh
```

Get the autocompletion scripts for zsh from this [repository](https://github.com/ahmetb/kubectx/tree/master/completion).

### _kubectx

```
#compdef kubectx kctx=kubectx

local KUBECTX="${HOME}/.kube/kubectx"
PREV=""

local context_array=("${(@f)$(kubectl config get-contexts --output='name')}")
local all_contexts=(\'${^context_array}\')

if [ -f "$KUBECTX" ]; then
    # show '-' only if there's a saved previous context
    local PREV=$(cat "${KUBECTX}")

    _arguments \
      "-d:*: :(${all_contexts})" \
      "(- *): :(- ${all_contexts})"
else
    _arguments \
      "-d:*: :(${all_contexts})" \
      "(- *): :(${all_contexts})"
fi
```

### _kubens
```
#compdef kubens kns=kubens
_arguments "1: :(- $(kubectl get namespaces -o=jsonpath='{range .items[*].metadata.name}{@}{"\n"}{end}'))"
```
