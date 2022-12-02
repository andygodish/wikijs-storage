---
title: Register a Service (Namespace)
description: How to register a namespace in a new Azure subscription.
published: true
date: 2022-12-02T17:30:52.886Z
tags: azure
editor: markdown
dateCreated: 2022-12-02T17:30:52.886Z
---

# Register a Service (Namespace)

When attempting to use terraform to spin up infrastructure in a newly created subscription, I have been met with an error similar to this: 

```
Message="The subscription is not registered to use namespace 'microsoft.insights'. See https://aka.ms/rps-not-found for how to register subscriptions."
 ```
 
These namespaces need to be registered via the `Resource Providers` portal from within your subscription. This space provides a list of all resource namespaces. Simply select the relevant ones to your infrastructure and click register. 

