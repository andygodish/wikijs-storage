---
title: Tagging
description: Tagging with git/GitHub
published: true
date: 2022-12-01T20:22:49.428Z
tags: github, git, cicd
editor: markdown
dateCreated: 2022-12-01T20:22:49.428Z
---

# Tagging

## Git CLI

You can associate a tag with a commit by running `git tag v<x.y.z>`. You can see your tag represented in the git logs, like so: 

```
# git log

commit d01ce2b25de2087ba5db7f7c4d16449f8d5ac9d8 (tag: v0.1.0)
Author: Andy Godish <33811239+andygodish@users.noreply.github.com>
```

You can then push your tags to a remote repository by running `git push --tags`. This is a separate push that locks the codebase at the associated commit. 

