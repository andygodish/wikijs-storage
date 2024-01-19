---
title: Local Wordpress Development
description: Tips and tricks for developing WP locally.
published: true
date: 2024-01-19T16:38:10.378Z
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

The wordpress container itself will automatically generate your wp-config.php based on environment variables set in the docker-compose.yaml, so you should immediately be able to access wp at localhost on the designated port.

### Database

Make sure to configure a version of mysql/mariadb that meets the requirements of WP. You can configure a database at container runtime by providing environment variables. For example: 

```
mysql:
  environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: my-wpdb
      MYSQL_USER: wpuser
      MYSQL_PASSWORD: password
```

The environment variables used by the `web service` (wordpress image) will need to match what you configure in the database container: 

```
web:
  environment:
      WORDPRESS_DB_HOST: mysql:3306
      WORDPRESS_DB_USER: wpuser
      WORDPRESS_DB_PASSWORD: password
      WORDPRESS_DB_NAME: my-wpdb
```

The DB host in the web service environment configuration can be set to `mysql` because each of these services are deployed to the same docker network. 

#### Network

This is configured in the docker-compose.yaml as well:

```
networks:
  wp-network:
    driver: bridge
```

It is assigned to each service:

```
mysql:
  networks:
  - wp-network
```

#### Database Volume

Instead of mounting the database directory to the local file system, the docker-compose yaml is configured to create a docker volume like so:

```
volumes: 
  db_data:
```

The docker volume, `db_data`, can then be mounted to the database directory configured in the container: 

```
mysql:
  volumes: 
    - db_data:/var/lib/mysql
```

## FS_Method

- [Stackoverflow Post](https://stackoverflow.com/questions/32073196/connection-information-wordpress-localhost-install)

Problem: During local development in a docker container, I attempted to install a plugin and was denied due to an error using FTP. Since I am developing locally, I do not have an FQDN for WP to, presumably, transfer files to. 

You can bypass this by adding the following to your `wp-config.php` file:

```
define( 'FS_METHOD', 'direct' );
```

I was then able to install the plugin directly. Make sure permissions are set correctly:

```
sudo chown -R www-data:www-data ./
```

## WordPress Code Separation

- [Reference Article](https://www.git-tower.com/blog/git-wordpress-2/)

The volumes in the docker-compose.yaml are designed to separate the `wp-content` folder from the core codebase of wordpress. This allows you to work on plugin and theme code in isolation from the version of wp defined by the image version of the `web` service (docker-compose.yaml).

This prevents the need to use an `.htaccess` file in the root directory to redirect traffic to another directory as outlined in the reference article above. 

## Remote-SSH (vscode)

Refer to this [Wiki Post]() outlining the vscode extension used to develop on the remote server running your containerized development environment. 

