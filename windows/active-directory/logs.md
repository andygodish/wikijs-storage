---
title: Viewing Logs
description: Viewing AD logs
published: true
date: 2023-10-15T21:33:48.548Z
tags: windows, sysadmin, active directory
editor: markdown
dateCreated: 2023-10-15T03:21:13.797Z
---

# AD Logs	

Login to your domain controller and open "Event Viewer."

Navigate to *Applications and Service Logs*:

![event_viewer.png](/images/event_viewer.png)

## Diagnostic Logging

By default, Active Directory records only critical events and error events in the Directory Service log. To configure Active Directory to record other events, you must increase the logging level by editing the registry.

- `HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\NTDS\Diagnostics` in Windows, specifically on an Active Directory Domain Controller, refers to a location in the Windows Registry where you can manage diagnostic logging levels for various Active Directory Domain Services (AD DS) events.
	- `HKEY_LOCAL_MACHINE`: A section of the registry that contains configuration information about the local computer.
	- `SYSTEM\CurrentControlSet\Services\NTDS`: Navigates through the registry tree to the services related to the NT Directory Service (NTDS, which is Active Directory).
  - `Diagnostics`: This subkey contains various entries that control the logging verbosity of different AD DS components.
  
1. win + r ---> regedit
2. Use the left nav to get to the diagnostics directory listed above

> Each entry (subkey) can be assigned a value from 0 through 5, and this value determines the level of detail of the events that are logged. The logging levels are described [here](https://learn.microsoft.com/en-us/troubleshoot/windows-server/identity/configure-ad-and-lds-event-logging#logging-levels)



