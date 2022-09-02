---
title: Quick Commands
description: Command for unzipping a compressed file.
published: true
date: 2022-09-02T18:58:30.383Z
tags: powershell
editor: markdown
dateCreated: 2022-08-18T04:14:26.102Z
---

# Powershell Quick Commands

## Unzip

This will unzip to your PWD. 

```
Expand-Archive .\Downloads\archived-files
```

You'll need to move that unzipped directory to your desired location (for example: "C:\curl") and then add the path to its binary file to your Path. 

## Get-Command

This will output the location of the binary for your CLI application. 

```
Get-Command docker
```

This will need to be in your path to work. The `cmd` equivelent would be `where docker`. On linux this is done with `which`

## ipconfig

Determine the ip address for a server running my network.

`ipconfig`

## New Item

This is equivalent to `touch` in linux. 

- [Reference Article](https://www.educative.io/answers/what-is-the-powershell-equivalent-of-touch)

```
ni <filename>.ps1
```