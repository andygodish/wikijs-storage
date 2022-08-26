---
title: DotNet Core Quick Commands
description: Provides a list of quick commands that you should know working with .Net Core.
published: true
date: 2022-08-26T15:08:22.943Z
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

## Add Packages to a Project

```
dotnet add package Microsoft.EntityFrameworkCore.InMemory
```

