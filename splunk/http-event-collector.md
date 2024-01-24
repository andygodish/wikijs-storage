---
title: Splunk - HEC
description: Basics on how to use Splunk  http event collectors.
published: true
date: 2024-01-24T04:46:36.828Z
tags: logging, splunk
editor: markdown
dateCreated: 2024-01-24T04:46:36.827Z
---

# Splunk - HEC

I worked a problem at work that involved using an Azure function app to forward logs coming into an Azure Eventn Hub. [This repository](https://github.com/splunk/azure-functions-splunk/tree/master/event-hubs-hec) was used primarily to build out the function app. It contains some useful information on the Event Hub as well. 

We use Splunk Cloud. The function app requires an HEC endpoint specific to our instance, those configurations look like this: 

```
"SPLUNK_HEC_TOKEN": "<your-token>",
"SPLUNK_HEC_URL": "https://<your-org>.splunkcloud.com/services/collector"
```