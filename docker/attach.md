---
title: Attach 
description: Using docker attach and associated flags to peak at a container's output stream. 
published: true
date: 2022-08-20T04:16:06.330Z
tags: docker
editor: markdown
dateCreated: 2022-08-20T04:16:06.330Z
---

# Docker Attach

- [Reference Article](https://docs.microsoft.com/en-us/dotnet/core/docker/build-container?tabs=linux)

After a container is running, you can connect to it to see the output. Use the docker start and docker attach commands to start the container and peek at the output stream. In this example, the Ctrl+C keystroke is used to detach from the running container. This keystroke will end the process in the container unless otherwise specified, which would stop the container. The --sig-proxy=false parameter ensures that Ctrl+C will not stop the process in the container.

After you detach from the container, reattach to verify that it's still running and counting.

```
docker start core-counter
core-counter

docker attach --sig-proxy=false core-counter
Counter: 7
Counter: 8
Counter: 9
^C

docker attach --sig-proxy=false core-counter
Counter: 17
Counter: 18
Counter: 19
^C
```
