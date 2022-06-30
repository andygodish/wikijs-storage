---
title: Install jq on RHEL
description: Quick referencing to installation guide(s) for jq. 
published: true
date: 2022-06-30T19:21:42.289Z
tags: kubernetes, linux, jq
editor: markdown
dateCreated: 2022-06-30T19:21:42.289Z
---

# Install jq on RHEL/Centos

Often I am googling the installation steps for jq when trying to troubleshoot various kubectl commands found in the wild. [This is a good reference that I usually find my way to](https://www.cyberithub.com/how-to-install-jq-json-processor-on-rhel-centos-7-8/).

## TLDR

```
yum install epel-release -y
yum update -y
yum install jq -y
```

## TODO

- Make a quick ansible playbook

