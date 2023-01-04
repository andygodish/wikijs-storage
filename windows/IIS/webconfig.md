---
title: Webconfig
description: Basics of the web.config file in IIS.
published: true
date: 2023-01-04T19:09:30.184Z
tags: windows, iis
editor: markdown
dateCreated: 2023-01-04T19:09:30.184Z
---

# Webconfig

In IIS, there is a configuration called called `web.config` located at the site level of a deployment made up of XML data. A lot of the data in this file appears to be bootstrapped as part of the installation process of a deployment. However, in my day-to-day, a corresponding file located in source control is updated by the developers to reflect the addition of new environment variables. Upon deploying a release, the CD tool used by my organization will sync the developer file with the actual web.config file on the deployment target. Adjacent to this process, environment variable values are also managed within the CD tool. The gist of it is as follows:

- Developers create the key by adding it to the web.config template in source control 
- This change is synced up and the new key(s) are added to the web.configs in the deployment targets
- At some point, the CD tool will apply env variable values corresponding to each key 
	- These are maintained by infrastructure based on the needs of the developers
- Keys + values can then be observed in the configuration file on the server

## Viewing Configuration Files

Start by opening IIS Manager from within the tools dropdown (top right) in Microsoft Server Manager. 

From the left sidbar you can see your server and all the deployed sites in the `Sites` directory. 

Right click on the site associated with a deployment and open the `web.config` file using Notepad. You'll see environment variables in this format:

```
<configuration>
  ...
  <appSettings> 
  ...
    <add key="yourKey" value="yourValue" />
```

### XPath Query

> XPath uses path expressions to select nodes or node-sets in an XML document. The node is selected by following a path or steps.

When managing variable values, we define the key using the entire xpath query. For example: 

```
# This is how the above key would be defined in our CD tool

/configuration/appSettings/add[@key='yourKey']/@value
```

