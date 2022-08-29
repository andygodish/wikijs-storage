---
title: Drive Mapping to a NAS Device
description: How-to on mapping a new drive to a shared folder on my QNAP NAS device. 
published: true
date: 2022-08-29T03:24:30.636Z
tags: windows, nas, storage
editor: markdown
dateCreated: 2022-08-29T03:24:30.636Z
---

# Mapping a Drive to a Windows Device

This quick tutorial will demonstrate how to map a drive on a windows machine to a shared folder on my QNAP NAS device running in the basement. 

First up, navigate to *This PC* in your windows file explorer. 

In the header of the file explorer window, click `Map a Network Drive.` I'm assigning mine `Q:` for QNAP. Enter the IP address (make sure this is static) of your NAS device preceded by `\\`. 

- If you have child directories as part of your QNAP shared folder, you can mount the drive directoy to a nester dir by appending the appropriate path to the server address.

Easy enough.