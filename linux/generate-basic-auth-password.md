---
title: Generate a Basic Auth Password
description: How to use apache2-utils to generate a basic auth password. 
published: true
date: 2023-01-02T16:54:48.131Z
tags: linux, appache2-utils, authentication
editor: markdown
dateCreated: 2023-01-02T16:54:48.131Z
---

# Generate a Basic Auth Password

## Install apache2-utils

```
sudo apt install apache2-utils
```

## Generate Your Username & Password

```
echo $(htpasswd -nb "<USER>" "<PASSWORD>") | sed -e s/\\$/\\$\\$/g
```

