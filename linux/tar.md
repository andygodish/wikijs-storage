---
title: Tar Command
description: Quick how-tos on using tar to extract files.
published: true
date: 2022-08-24T22:02:14.652Z
tags: linux, tar
editor: markdown
dateCreated: 2022-08-24T22:02:14.652Z
---

# Tar

## gzip: stdin: not in gzip format

- [Reference Article](https://itsfoss.com/how-solve-stdin-gzip-format/)

When attempting to unzip the `oc` package containing its binary, I was presented with this error: 

```
gzip: stdin: not in gzip format
```

To identify the format, use the `file` command:

```
file oc.tar
---
oc.tar: POSIX tar archive (GNU)
```

Since it was not a gzipped file, a simple tar is able to extract the file:

```
tar xvf oc.tar
```