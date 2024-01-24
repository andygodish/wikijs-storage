---
title: Testing Network Connectivity to Azure Resources
description: It's all in the title.
published: true
date: 2024-01-24T04:55:50.512Z
tags: azure, networking
editor: markdown
dateCreated: 2024-01-24T04:55:50.512Z
---

# Testing Network Connectivity to Azure Resources

At my work, our Azure resources live within out corporate network. While on the VPN, I can connect to these resources from my local machine over the network. Here are various tools I use to test connections to difference resources.

## Event Hubs/Namespaces

I have a function app that pushes diagnostic logs to an event hub. During local development of the function app, I was able to determine that I could reach the event hub namespace from my machine by running the following netcat/telnet commands:

```
nc -zv <event-hub-namespace-name>.servicebus.windows.net 443

telnet <event-hub-namespace-name>.servicebus.windows.net 443
```

The portion of the URL .servicebus.windows.net you're referring to is specific to Azure Service Bus and related services, which include Azure Event Hubs. This domain is used for addressing Service Bus resources, including namespaces. Different Azure services use different patterns for their FQDNs. 

