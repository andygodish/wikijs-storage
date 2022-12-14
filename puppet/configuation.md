---
title: Puppet Configuration
description: All about the puppet configuration file. 
published: true
date: 2022-12-14T21:53:10.586Z
tags: puppet, configuration-management
editor: markdown
dateCreated: 2022-12-14T21:42:07.571Z
---

# Puppet Configuration

## /etc/puppetlabs/puppet/puppet.conf

Following installation, the configuration file for puppet and puppet server can be found here: 

```
/etc/puppetlabs/puppet/puppet.conf
```

## /etc/default/puppetserver

```
vi /etc/default/puppetserver
```

Find the line starting with the key, `JAVA_ARGS=`. You can update this to tell the server to use less memory if running on a smaller server. 

*example:* change the instances of `2g` to `1g` or `512m`, etc; `JAVA_ARGS=-Xms512m -Xmx512m...`