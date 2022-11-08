---
title: Octopus Parameters
description: Short how-to on Octopus parameters
published: true
date: 2022-11-08T03:25:08.500Z
tags: octopus, cicd
editor: markdown
dateCreated: 2022-11-08T03:25:08.500Z
---

# Parameters

This exisitng process step was encountered during a deployment: 

```
$photouploadurl = $OctopusParameters["Photo.Upload.Url"]
$pathToJson = $OctopusParameters["Photo.Upload.Directory"]
```

The parameters in this case were just variables set under the `Project Variables` variable subsection. 