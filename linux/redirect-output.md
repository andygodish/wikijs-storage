---
title: Redirecting Output
description: Quick how-tos on redirecting output in various scenarios.
published: true
date: 2022-08-18T04:11:21.847Z
tags: bash, linux, command line
editor: markdown
dateCreated: 2022-08-17T03:14:56.730Z
---

# Redirecting Output	

## STDERR

Scenario: Querying kubectl json output using jq. When a command does not output anything, jq spits out an error: 

```
jq: error (at <stdin>:2606): Cannot iterate over null (null)
```

I often run for loops against `-o json` kubectl outputs that result in a series of these errors being outputted in my terminal. Remove them by directing STDERR to `/dev/null` like so,

```
k get clusterrolebinding -o json | jq -r '.items[] | select((.subjects[].name=="default") and (.subjects[].kind=="ServiceAccount"))' 2>/dev/null
```

Where `2>/dev/null` is the important bit. 