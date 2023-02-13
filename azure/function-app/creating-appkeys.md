---
title: Creating App Keys
description: I ran into an issue pertaining to FW rules that would allow my function app to create and store app keys to an associated storage account resource. 
published: true
date: 2023-02-13T16:16:12.074Z
tags: azure, function-app
editor: markdown
dateCreated: 2023-02-13T16:16:12.074Z
---

# Creating App Keys

An Azure Function App always has an associated storage account. In order to write the app keys to the storage account utilizing a sub-target type of `file`, ports **445** and **80** need to be open between the outbound CIDR of the function app and the PE endpoint of the storage account. 