---
title: Changing the Entrypoint 
description: How to override the entrypoint command of a dockerfile. 
published: true
date: 2023-02-01T04:43:31.650Z
tags: docker
editor: markdown
dateCreated: 2023-02-01T04:43:31.650Z
---

# Changing the Entrypoint

- [Reference Article](https://learn.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=linux#change-the-entrypoint)

## Change the ENTRYPOINT
The docker run command also lets you modify the ENTRYPOINT command from the Dockerfile and run something else, but only for that container. For example, use the following command to run bash or cmd.exe. Edit the command as necessary.


In this example, the ENTRYPOINT is changed to bash. The exit command is run which ends the process and stop the container.

````
docker run -it --rm --entrypoint "bash" counter-image
root@9f8de8fbd4a8:/App# ls
DotNet.Docker  DotNet.Docker.deps.json  DotNet.Docker.dll  DotNet.Docker.pdb  DotNet.Docker.runtimeconfig.json
root@9f8de8fbd4a8:/App# dotnet DotNet.Docker.dll 7
Counter: 1
Counter: 2
Counter: 3
^C
root@9f8de8fbd4a8:/App# exit
exit
```
