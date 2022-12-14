---
title: Puppet Configuration
description: All about the puppet configuration file. 
published: true
date: 2022-12-14T22:11:12.983Z
tags: puppet, configuration-management
editor: markdown
dateCreated: 2022-12-14T21:42:07.571Z
---

# Puppet Configuration

## Server Configuration

### Configuration File

Following installation, the configuration file for puppet and puppet server can be found here: 

```
/etc/puppetlabs/puppet/puppet.conf
```

### Default Puppet Server Configuration File

```
vi /etc/default/puppetserver
```

Find the line starting with the key, `JAVA_ARGS=`. You can update this to tell the server to use less memory if running on a smaller server. 

*example:* change the instances of `2g` to `1g` or `512m`, etc; `JAVA_ARGS=-Xms512m -Xmx512m...`

### CA Setup

Before you can access the `puppetserver` cli, you need to start the service for the first time - it will be added to your path automatically. 

Access the CLI beforehand to perform your CA setup at `/opt/puppetlabs/bin/puppetserver ca setup`. 

`puppetserver ca list` can be used to view CAs

### Start PuppetServer

```
systemctl start puppetserver
```
- memory issues will present here