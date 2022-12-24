---
title: Set
description: Using the set command. 
published: true
date: 2022-12-24T00:07:51.262Z
tags: bash, linux, scripting
editor: markdown
dateCreated: 2022-12-24T00:07:51.262Z
---

# Set 


## Examples

`set -eu`:

```
#!/bin/sh
set -eu
```

The -e option causes the script to exit immediately if any command in the script returns a non-zero exit status. This is useful for catching errors in the script and handling them appropriately.

The -u option causes the script to exit immediately if it tries to use an unset variable. This is useful for preventing the script from using undefined variables, which can lead to unexpected behavior.

By enabling these options, the set -eu line helps to ensure that the script will run smoothly and predictably, as it will exit immediately if any errors or undefined variables are encountered.