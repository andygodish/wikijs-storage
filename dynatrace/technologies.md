---
title: Technologies & Processes
description: Overview of Technologies & Processes in Dynatrace
published: true
date: 2024-01-30T04:41:38.211Z
tags: dynatrace, monitoring
editor: markdown
dateCreated: 2024-01-30T04:41:38.211Z
---

# Technologies & Processes

Various application and services use some kind of forwarding agent in the form of a product offered by Dynatrace. For example, OneAgent is installed within container images deployed to Azure, and ActiveGate/Dynacube is used in our OpenShift environments. 

These services are compiled into a single dashboard within our Dynatrace cloud instance under `Technologies & Processes` within the Infrastructure Observability section of the left side panel.

![tech-and-processes.png](/images/tech-and-processes.png)

Find your app in the dashboard to view metrics associated with its associated infrastructure. 

## Issue Following an Upgrade

It was reported that a single microservice was failing to present data on a dashboard visual. Investigation revealed a recent update to a K8s cluster the previous week. I suspect that the process restarted this service's pods before  the Dynatrace forwarding agent pods were up and running. This may have resulted in DT not reading in the container on its associated k8s node. 

Within the app's page from the `Technologies & Processes` dashboard, I scrolled down to the bottom and noticed the "Restart Pending" next to each registered pod:

![dt-restart-pending.png](/images/dt-restart-pending.png)

To fix this, I needed to perform a `rollout restart` of the deployment. That resulted in data being populated on the dashboard and removed the "Restart Pending" alert. 