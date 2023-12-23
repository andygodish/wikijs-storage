---
title: Local Wordpress Development
description: Tips and tricks for developing WP locally.
published: true
date: 2023-12-23T16:54:07.423Z
tags: development, wordpress
editor: markdown
dateCreated: 2023-12-16T17:59:08.482Z
---

# Local Wordpress Development

This document outlines my process for developing in Wordpress locally using docker containers.

## FS_Method

- [Stackoverflow Post](https://stackoverflow.com/questions/32073196/connection-information-wordpress-localhost-install)

Problem: During local development in a docker container, I attempted to install a plugin and was denied due to an error using FTP. Since I am developing locally, I do not have an FQDN for WP to, presumably, transfer files to. 

You can bypass this by adding the following to your `wp-config.php` file:

```
define( 'FS_METHOD', 'direct' );
```

I was then able to install the plugin directly. Make sure permissions are set correctly:

```
sudo -u root touch /var/www/html/wordpress/wp-content/plugins/test.txt
```