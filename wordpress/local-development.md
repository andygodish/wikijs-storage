---
title: Local Wordpress Development
description: Tips and tricks for developing WP locally.
published: true
date: 2023-12-23T19:46:15.397Z
tags: development, wordpress
editor: markdown
dateCreated: 2023-12-16T17:59:08.482Z
---

# Local Wordpress Development

- [Repository](https://github.com/andygodish/wordpress-dev)
- [Documentation](https://github.com/andygodish/wikijs-storage/blob/main/wordpress/local-development.md)

This document outlines my process for developing in Wordpress locally using docker containers. The focus is proper configuration of the WP source code, not theme and plugin development. 

## Docker Compose

A docker compose file containing the manifest for the deployment of 4 services. The first two container images meet the requirements listed in the [WordPress docs](https://developer.wordpress.org/advanced-administration/before-install/#requirements-on-the-server-side).

- wordpress
- mysql
- phpmyadmin
- wordpress:cli-*-php*

The phpmyadmin and wordpress cli (from the same wordpress image) are nice to haves for development and learning your way around wordpress, but are not required to deploy a working instance of WP itself. 

### Wordpress (web service)

The `web` service listed deploys the wordpress UI application.

#### Volume

The WP application code for the web service is served by an apache instance built into the container image by default. Apache looks to serve the files at `/var/www/html/`. Running the container with this volume mount will copy the files from from that directory in the container to the root directory of your repo. This negates the need to download the source files outlined in the [installation docs](https://developer.wordpress.org/advanced-administration/before-install/howto-install/#basic-instructions).

The wordpress container itself will automatically generate your wp-config.php based on environment variables set in the docker-compose.yaml.

### Database



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