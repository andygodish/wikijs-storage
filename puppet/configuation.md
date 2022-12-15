---
title: Puppet Configuration
description: All about the puppet configuration file. 
published: true
date: 2022-12-15T04:07:43.143Z
tags: puppet, configuration-management
editor: markdown
dateCreated: 2022-12-14T21:42:07.571Z
---

# Puppet Configuration
- [Installation Docs](https://puppet.com/docs/puppet/7/install_agents.html#install_agents)
- [Installation Guide for Centos/RHEL8](https://computingforgeeks.com/install-puppet-master-and-agent-on-centos-rhel-8/)
- [NTP Configuration](https://computingforgeeks.com/how-to-configure-ntp-server-using-chrony-on-rhel-8/)

## Server Configuration

- I couldn't get this installed on ubuntu 22.04, but centos 8 worked

### Configuration File

Following installation, the configuration file for puppet and puppet server can be found here: 

```
/etc/puppetlabs/puppet/puppet.conf
```

### Default Puppet Server Configuration File
- [JVM Tuning](https://puppet.com/docs/puppet/7/server/install_from_packages.html#running-puppet-server-on-a-vm)

```
vi /etc/default/puppetserver
```

Find the line starting with the key, `JAVA_ARGS=`. You can update this to tell the server to use less memory if running on a smaller server. 

*example:* change the instances of `2g` to `1g` or `512m`, etc; `JAVA_ARGS=-Xms512m -Xmx512m...`

### Example Server Config

```
[master]
dns_alt_names = puppetserver,puppetserver.lan.andyg.io,puppetserver01

[main]
certname = puppetserver.lan.andyg.io
server = pupperserver.lan.andyg.io
environment = production

[server]
vardir = /opt/puppetlabs/server/data/puppetserver
logdir = /var/log/puppetlabs/puppetserver
rundir = /var/run/puppetlabs/puppetserver
pidfile = /var/run/puppetlabs/puppetserver/puppetserver.pid
codedir = /etc/puppetlabs/code
```

### CA Setup

Before you can access the `puppetserver` cli, you need to start the service for the first time - it will be added to your path automatically. 

Access the CLI beforehand to perform your CA setup at `/opt/puppetlabs/bin/puppetserver ca setup`. 

`puppetserver ca list` can be used to view CAs

### Start PuppetServer

```
systemctl start puppetserver
```
- memory issues will present here

## Puppet Agent

### Server Configuration

In the same `puppet.conf` configuration file used for server configuration, create a `[main]` block with a `server = puppetserver.com` configuration set to the hostname of your puppet server. 

You can also access the `puppet` cli in a similar manner as the puppet server before the intial startup of the service to set this configuration value. 

### Corresponding Puppet Agent Configuration

```
[main]
certname = puppetagent.lan.andyg.io
server = puppetserver.lan.andyg.io
environment = production
```

### Signing Certificate for Agent

Run this command on your agent node to populate the ca and SHA256 associated with the agent in the server's `ca list`,

```
puppet agent --test --ca_server=puppetmaster.computingforgeeks.com
```

You'll see an error with this command, but upon logging back into your server node, you can see this certificate request by running, 

```
puppetserver ca list
---

Requested Certificates:
    puppetagent.lan.andyg.io       (SHA256)  C4:F8:D9:8E:5F...
```

Finally, sign it using the following command: 

```
puppetserver ca sign --certname puppetagent.lan.andyg.io
```