---
title: OpenShift Quick Commands
description: Quick commands page for OpenShift.
published: true
date: 2022-08-29T15:20:15.283Z
tags: kubernetes, openshift
editor: markdown
dateCreated: 2022-08-29T15:20:15.283Z
---

# OpenShift	

## Add Administrator Role to a User Account

Get your list of users by running the following `oc` command: 

```
oc get users
```

The `oc` tool contains a helpful set of administrative commands accessed via the `adm` flag. See `oc adm -h` for more detailed information. I believe this is creating a rolebinding and adding it to the user account. 

```
oc adm policy add-cluster-role-to-user <role (cluster-admin)> <user>
```