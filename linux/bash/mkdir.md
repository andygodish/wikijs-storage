---
title: mkdir
description: Running mkdir command and options. 
published: true
date: 2022-12-24T01:21:20.782Z
tags: bash, linux, scripting
editor: markdown
dateCreated: 2022-12-24T01:21:20.782Z
---

# mkdir

## Examples

```
mkdir -m 0777 -p "$VOL_DIR"
```

In the mkdir command, the -m option is used to set the permissions on the directory that is being created.

The 0777 value following the -m option specifies the permissions that will be set on the directory. In this case, the permissions will allow all users (owner, group, and others) to read, write, and execute the directory.

The -p option tells mkdir to create any intermediate directories that do not already exist, if necessary. This means that if the directories leading up to "$VOL_DIR" do not exist, mkdir will create them with the specified permissions.

For example, if "$VOL_DIR" is "/tmp/mydir/subdir" and the directories "/tmp" and "/tmp/mydir" already exist, mkdir will create the "/tmp/mydir/subdir" directory with the specified permissions. If the "/tmp/mydir" directory does not exist, mkdir will create it and all intermediate directories with the specified permissions.