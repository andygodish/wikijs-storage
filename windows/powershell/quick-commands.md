---
title: Quick Commands
description: Command for unzipping a compressed file.
published: true
date: 2022-09-02T19:35:36.361Z
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

## Write-Output
- Similar to `echo` in linux - `Write-Output` is aliased to echo by default

## Out-File

Cmdlet used to direct PS output to a file

```
Get-Process | Out-File <filename>.txt
```

## Get-Command

This will output the location of the binary for your CLI application. 

```
Get-Command docker
```

This will need to be in your path to work. The `cmd` equivelent would be `where docker`. On linux this is done with `which`

## Get-Process

- Similar to `ps` in linux

## ipconfig

Determine the ip address for a server running my network.

`ipconfig`

## New Item

This is equivalent to `touch` in linux. 

- [Reference Article](https://www.educative.io/answers/what-is-the-powershell-equivalent-of-touch)

```
ni <filename>.ps1
```

## Piping Example

```
 Get-Process | where {$_.CPU -gt 1000} | Out-File cpu.txt
```
- Unclear on how the where command works with formatted output you get from a PS cmdlet like `Get-Process`
