---
title: Puppet Modules
description: Basics of Puppet Modules
published: true
date: 2022-12-15T04:58:56.769Z
tags: linux, puppet, configuration-management
editor: markdown
dateCreated: 2022-12-15T03:14:14.985Z
---

# Puppet Modules

Module location --- `/etc/puppetlabs/code`

The default environment is *production* - build an example nginx module in here as part of this example, you'll see a modules directory in here as well. 

environment --> module --> manifest --> class --> resource types --> attributres

Use the *puppet development kit* (pdk) to create the scafolding of a module module in your modules directory.

```
sudo rpm -Uvh https://yum.puppet.com/puppet-tools-release-el-8.noarch.rpm
sudo yum install pdk
```

Use the binary to create a new module called nginx: 

```
/opt/puppetlabs/pdk/bin/pdk new module nginx
```

A folder with the name of your module will be created with a default set of files and folders. You don't need to worry about all of them.

- files(dir) - files that need to go to the nodes
- manifests - configuration files
- templates - like files, but templates used to generate content on nodes

## Create a Manifest

Where one modules is meant to configure a single service, a manifest is a file (.pp) that stores `class definitions`. A class is one unit of the module installation. 

From within your modules, run a `pdk new class <name>` to create a new class called *\<name>.pp* in your manifests directory. NOTE: this must be run alongside the `manifests.json` file bootstrapped by pdk in the module. 

## Build Manifest using Resource Types

### package 

- use double quotes when ther is a parameter that needs to be parsed.
- `=>` is a hash rocket

```
class <module>::<manifest> {
  package {'install_nginx': <--- just a name
    name   => nginx,        <--- this is an attribute
    ensure => 'present',
  }
}
```

Use `puppet parser validate install.pp` to lint your file. No return is good. 

## Map Class back to Module --> Module to Agent Nodes 

init.pp & site.pp files perform this function.  

### init.pp Class

Every module must have one of these. This is called when a module is referenced. 

Use pdk to create a new class with the same name as the module in order to produce an init.pp file in your module's manifest dir.

```
pdk new class nginx
```

In the nginx class, you simply need to attach all the manifests that you wish to be executed by the module. 

```
class nginx {
  contain nginx::install
}
```

## Call the Module 

In you environment manifests directory (up from the module-level), you need to create a site.pp. 

No pdk command for this one, just create a *site.pp* file:

```
node <nodename-on-file> {
  class { 'nginx': }
} 
```

- look up how to find agent nodes registered with the puppet server

### Force a Puppet Run

Tell puppet to run a *catalog converge* (merging of all modules mapped to the node, right meow).

From the agent node: 

```
puppet agent -t
```



