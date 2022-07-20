---
title: Brace Expansion
description: Quick how-to on brace expansion. 
published: true
date: 2022-07-20T19:59:08.318Z
tags: bash, brace-expansion
editor: markdown
dateCreated: 2022-07-20T19:54:42.554Z
---

# Brace Expansion	

Create multiple files in sequential order from 1 to 3 with the following command:

`touch bspl{0001..0003}.c`

The ugly code that follows (bspl{0001..0003}.c) is called a brace expansion. This is a bash shell feature that allows you to create long lists of arbitrary string combinations. 

This will result in the following files in your pwd:

```
bspl0001.c bspl0002.c bspl0003.c
```