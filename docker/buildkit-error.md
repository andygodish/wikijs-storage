---
title: Buildkit Error
description: Troubleshooting a Docker buildkit error I encountered at work. 
published: true
date: 2024-01-22T22:14:55.942Z
tags: docker, troubleshooting
editor: markdown
dateCreated: 2024-01-22T22:14:22.468Z
---

# Docker Buildkit Error

While attempting to build a Dockerfile on self-hosted runner at work, I enounted the following error: 

![buildkit-error.png](/images/buildkit-error.png)

I was able to fix the issue by disabling docker docker buildkit by appending `BUILDKIT=0` infront of my build command:

```
DOCKER_BUILDKIT=0 docker build .
```

## Explanation

Using DOCKER_BUILDKIT=0 before your Docker build command disables the BuildKit feature in Docker. BuildKit is an advanced image-building engine introduced in Docker 18.09, offering several improvements over the traditional Docker image build process. However, it can sometimes lead to compatibility issues or unexpected behavior due to its different way of handling build contexts and layers.

The app inquestion is a dotnet7 app. I haven't run into this issues on any other projects, it is possible that it is an issue with this tech stack or the version of docker originally used by the dev team to build the original Dockerfile. 

By setting DOCKER_BUILDKIT=0, Docker ignores the newer BuildKit features and revert to the traditional build process. This can resolve issues stemming from BuildKit's different handling of Dockerfile instructions, file copying mechanisms, layer caching, and error reporting.

