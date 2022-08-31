---
title: DotNet Core Quick Commands
description: Provides a list of quick commands that you should know working with .Net Core.
published: true
date: 2022-08-31T05:26:26.383Z
tags: dotnet
editor: markdown
dateCreated: 2022-08-26T15:08:22.943Z
---

# .Net Core	

## Scaffolding a New ASP.Net Project

```
dotnet new webapi -n <NameOfProject>
```

The `webapi` portion of the command is what scaffolds a *Core* project. 

> In ASP.NET Core, there's no longer any distinction between MVC and Web APIs. There's only ASP.NET MVC, which includes support for view-based scenarios, API endpoints, and Razor Pages (and other variations like health checks and SignalR). In addition to being consistent and unified within ASP.NET Core, APIs built in 

### Minimal API

```
dotnet new webapi -minimal -n <Name>
```

## Add Packages to a Project

```
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

## Secret Store

You can initialize a "built-in" secret store by running `dotnet user-secrets init`. Look more into where/how these secrets are stored/accessed. 

Once initialized, run the following command to add secrets to the store:

```
dotnet user-secrets set "Key" "value" 
```

## Weird Bug (VsCode)

- [Reference Post](https://stackoverflow.com/questions/7035437/how-to-fix-namespace-x-already-contains-a-definition-for-x-error-happened-aft)

#### TLDR

Looks like a bug in VS code's `OmniSharp`.

Solution for me was to execute command "Restart OmniSharp". In VsCode:

```
ctrl + shift + p
---
Restart OmniSharp
```

## Run Your Application

In the root of your scaffolded application:

```
dotnet run
```

## Perform a DB Migration

```
dotnet-ef migrations add initialmigration
```
- Where I believe the last portion is a name given by the user

You'll need to install the dotnet-ef tool gloabally and add your `~/.dotnet/tools/` directory to your path. 

```
dotnet tool install --global dotnet-ef
```

Note that you've only created migratins at this point. You will still need to run them against your database. You'll see a `Migrations` in the root directory of your project. To apply your migrations, run the following: 

```
dotnet-ef database update
```

