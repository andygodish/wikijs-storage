---
title: Publishing a Release
description: Notes on publishing releases to github
published: true
date: 2022-10-09T04:56:15.874Z
tags: github, cicd
editor: markdown
dateCreated: 2022-10-09T04:56:15.874Z
---

# Publish a Release

You will probably want to your build step to follow some sort of linting step that won't require a large package download to complete. With this in mind, you are introducing a dependency to your build `step`. You can insure that a step won't kick off until a specified step is completed by using the `job.needs` in your workflow yaml:

```
job: build
  needs: lint     ***
  steps:
  ...
```

## Release Asset

There are situations where you would want to add a release asset to your release. 

In a previous job, a build artifact will likely have already been built. This "build artifact" will have been archived or uploaded (probably more to this) so that it can be referenced in subsequent steps. 

- This may have been accomplished with `actions/upload-artifact@v2`.



