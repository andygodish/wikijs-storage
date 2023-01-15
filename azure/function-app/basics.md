---
title: Function App
description: Getting started with Azure Function Apps. 
published: true
date: 2023-01-15T21:46:33.351Z
tags: azure, function-app, serverless
editor: markdown
dateCreated: 2023-01-15T20:13:52.860Z
---

# Azure Function Apps

Just outlining a simple dotnet demonstration app involving the containerization and deployment of an Azure Function App that fetches the current price of Bitcoin. 

- Following [this tutorial](https://learn.microsoft.com/en-us/azure/azure-functions/functions-create-function-linux-custom-image?tabs=in-process%2Cbash%2Cazure-cli&pivots=programming-language-csharp)

## Scaffold the Project

First, create and cd your project directory. 

```
mkdir BitcoinPrice && cd ./BitcoinPrice
```

Next, Initialize the function app with the following command: 

```
func init --worker-runtime dotnet --docker
```

This leaves you with a valid Dockerfile that allows for the utilization of Azure function apps. 

Finally, create the base function app c# code by running the following command: 

```
func new --name HttpExample --template "HTTP trigger" --authlevel anonymous
```

## Application Code

In order to make an HTTP request to an external API, we need to utilize the `System.Net.http` namespace and create a new client object which can leverage the `GetAsync` method. 

```
using System.Net.Http;
---
HttpClient client = new HttpClient()
var response = await client.GetAsync("https://api.coinbase.com/v2/prices/BTC-USD/sell/")
```