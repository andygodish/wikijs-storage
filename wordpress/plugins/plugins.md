---
title: Plugins
description: Basics about WP Plugins
published: true
date: 2024-01-02T16:09:43.030Z
tags: wordpress, plugins
editor: markdown
dateCreated: 2023-10-22T22:11:02.531Z
---

# Plugins

Plugins are installed in the `wp-content/plugins` directory on the local file system. 

Plugins can be activated/deactivated. 

- How is this done in the codebase?

## Plugins

Tracking plugins for use in the future

- Advanced Custom Fields (Free & Pro Version)
- Classic Editor
- Contact Form 7
- Copy & Delete Posts

### Advanced Custom Fields

This will remove the default custom fields functionality that comes with WP out of the box. Note: existing custom fields data will be maintained after installation. This data will still be pulled from the database. 

You'll have to use a plugn specific function, 

```
<?php the_field('your-field');?>
```

#### Repeater Field

- Pro version is needed for this feature of Advanced Custom Fields

Adding a list of sub fields into an array. 


