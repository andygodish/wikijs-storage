---
title: Windows Services
description: How to manage services running on Windows servers.
published: true
date: 2022-12-27T17:50:47.502Z
tags: windows, services, sysadmin
editor: markdown
dateCreated: 2022-12-27T17:50:47.502Z
---

# Windows Servers

To view a list of running services on a Windows machine, you can use the following methods:

Use the **services.msc** snap-in:

- Press the **Windows key + R** to open the **Run dialog**.
- Type **services.msc** and press Enter.

This will open the Services window, which lists all the services installed on the machine. The Status column shows the current status of each service (e.g., Running, Stopped).

Use the sc command:

- Open a command prompt or PowerShell window.
- Type `sc` query and press Enter.

This will display a list of all the services installed on the machine, along with their current status.

Use the Task Manager:

- Press the Ctrl + Alt + Del keys at the same time to open the security screen.
- Click the Task Manager button.
- In the Task Manager window, click the Services tab.

This will show a list of all the services that are running on the machine.

Use the net command:

- Open a command prompt or PowerShell window.
- Type net start and press Enter.

This will display a list of all the services that are currently running on the machine.
You can also use the Tasklist command in a command prompt or PowerShell window to view a list of running processes and their associated services. To do this, type tasklist and press Enter.