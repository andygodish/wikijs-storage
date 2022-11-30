---
title: MSSQL Server
description: Deploy a Microsoft SQL Server for development purposes. 
published: true
date: 2022-11-30T18:51:50.438Z
tags: docker, sql server, dev
editor: markdown
dateCreated: 2022-11-30T18:51:50.438Z
---

# Microsoft SQL Server

Deploy a MS SQL server locally using docker compose. The environment variables are pretty straight forward. You can demonstrate that this worked by connecting to it locally using Microsoft SQL Server Management Studio with the username of SA - which I believe is a default admin user.

> I use WSL (Ubuntu) on my local Windows machine. When I launch this container using docker in my Ubuntu environment and try to connect MSQLMS to it, the localhost address does not work. I am not sure what IP address I need to use to connect to this from the Windows environment itself. 

```
# docker-compose.yaml

version: '3'
services: 
  sqlserver: 
    image: "mcr.microsoft.com/mssql/server:2019-latest"
    environment: 
      ACCEPT_EULA: "Y"
      SA_PASSWORD: "pa55w0rd!"
      MSSQL_PID: "Express"
    ports: 
    - "1433:1433"
```

