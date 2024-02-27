---
title: File Exploration in Windows
description: File Exploration in Windows
published: true
date: 2024-02-27T14:45:56.220Z
tags: windows, filesystem
editor: markdown
dateCreated: 2024-02-27T14:45:56.220Z
---

# File Exploration in Windows

I worked a ticket that stemmed from a low drive space warning received in Solarwinds. The server in question was a Windows server. 

## File Explorer

`File Explorer` --> `This PC` --> `Devices & Drives`

The drive in question was `D:`...this was derived from SolarWinds.

When I entered the directory, each directory size was not immeditely available. I could hover over a single directory at a time and see a tooltip populate with the information.

## Powershell

Instead, run the following for a similar experience to running `ls -lah` on a linux machine: 

```
Get-ChildItem -Directory | ForEach-Object {
    $folder = $_
    $size = (Get-ChildItem $folder.FullName -Recurse -File | Measure-Object -Property Length -Sum).Sum
    [PSCustomObject]@{
        Name = $folder.Name
        Size = $size
        SizeInMB = [math]::round($size / 1MB, 2)
    }
} | Sort-Object Size -Descending | Format-Table -AutoSize
```

`Get-ChildItem` is aliased to `ls` in powershell. 