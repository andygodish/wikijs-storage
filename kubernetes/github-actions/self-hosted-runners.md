---
title: Managing Self Hosted Github Runners
description: Tidbits about managing self hosted github actions runners. 
published: true
date: 2023-01-26T05:35:44.701Z
tags: github, cicd, github actions
editor: markdown
dateCreated: 2023-01-26T05:30:10.862Z
---

# Managing Self Hosted Github Runners

I wrote an unanswered issue in the upstream repository for the openshift maintained runner images summarizing an issue that I am seeing when these runner pods run workloads for an extended period of time. More information can be assertained by reading the issue itself [here](https://github.com/redhat-actions/openshift-actions-runners/issues/23). 

There are two primary issues that I encountered, one is that the use of the redhat public actions break the configurations specific to the OpenShift cluster I use at work, and the second appears to be some kind of caching that is taking place in the `.local` directory on each pod's filesystem. I haven't confirmed that this is in fact the issue, but troubshooting efforts that involve wiping that directory, or the overaching pod, result in a functioning runner. 

To get around this issue, my plan is to utilize the `base` image found in the same OpenShift repo mentioned above as an intermediary runner that would simply spin up short lasting runners with more application specific tools needed by a development team. 

