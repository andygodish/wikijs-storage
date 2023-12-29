---
title: WordPress Image Sizing
description: Notes on the built in WP features involving image sizing. 
published: true
date: 2023-12-29T06:33:10.711Z
tags: wordpress, media, images
editor: markdown
dateCreated: 2023-12-29T06:29:19.495Z
---

# WordPress Image Sizing

## Custom Image Sizes

Navigate to your `functions.php` file and add the following:

where, 

- the name given is "blog-large"
- 800px wide
- 400px high
- and with the `hard crop` setting set to true
	> Say you upload an image that is 1000px by 500px, WP will automatically crop it to 800x500. Note, WP will not take into consideration the aspect ratio of the image. Meaning, it is just going to cut out the smaller dimension from your uploaded image. 

```
add_image_size('blog-large', 800, 400, true)
```

### Applying Image Sizes to Existing Images

#### Plugin

**Force Regenerate Thumbnails**

Search for the above text in the plugin store. You're looking for this plugin: 

![force-regenerate-plugin.png](/images/force-regenerate-plugin.png)

This is useful if you already have a large number of images uploaded. You can also simple delete and upload images again.

Now you can tell your php code to use a particular image size as needed: 

```
<?php if(has_post_thumbnail()):?>
	  <img src="<?php the_post_thumbnail_url('blog-large');?>"/>
	<?php endif;?>
```