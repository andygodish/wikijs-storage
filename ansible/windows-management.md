---
title: Manage Windows Servers with Ansible
description: Overview on how to configure Ansible correctly to manage a Windows server. 
published: true
date: 2022-09-01T14:26:07.675Z
tags: ansible, windows
editor: markdown
dateCreated: 2022-09-01T14:26:07.675Z
---

# Manage Windows Servers with Ansible

WinRM is a management protocol used by Windows to remotely communicate with another server. It is a SOAP-based protocol that communicates over HTTP/HTTPS, and is included in all recent Windows operating systems. Since Windows Server 2012, WinRM has been enabled by default, but in most cases extra configuration is required to use WinRM with Ansible.

## pywinrm

Ansible uses the pywinrm package to communicate with Windows servers over WinRM. It is not installed by default with the Ansible package, but can be installed by running the following:

```
pip install "pywinrm>=0.3.0"

```

## ansible_winrm_transport variable

When connecting to a Windows host, there are several different options that can be used when authenticating with an account. The authentication type may be set on inventory hosts or groups with the ansible_winrm_transport variable.