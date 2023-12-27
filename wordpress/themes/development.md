---
title: WordPress Theme Development
description: WordPress theme development
published: true
date: 2023-12-27T18:23:28.915Z
tags: dev, wordpress, themes
editor: markdown
dateCreated: 2023-12-27T05:37:44.888Z
---

# WordPress Theme Development

- [Repository]()
- [Documentation]()

## TailPress

- [Github Readme/Documentation](https://github.com/jeffreyvr/tailpress/#readme)

I chose this open source template due to my past experience with Tailwind css. This repo is actively maintained at the time of this writing and does come with a working bootstrap of Tailwind. 

I follow the "Regular Method" installation process involving the cloning of the TailPress repo as a starter template. There is a lot of npm dependencies that I have been ignoring without issue. From what I can tell, these only pertain to development and shouldn't have any bearing on production builds of the theme itself. 

- [Regular Method](https://github.com/jeffreyvr/tailpress/?tab=readme-ov-file#regular-method)

Using the local development environment outlined [here](https://github.com/andygodish/wikijs-storage/blob/main/wordpress/local-development.md), I can simply clone the theme repo into the `wp-content/themes` directory of my WP source code. It is then available for selection as a theme in the admin console.

### Install

In order to properly install tailwind and the other dev dependencies that come with TailPress, run the following `npm` commands from the root of the cloned:
- `npm install` 
- `npm run dev`
- `npm run watch`

Out of the box, you are not going to have access to the full tailwind codebase. For example, attempting to work with a new color (ie, `text-red-900`) in your code base will not work until `npm run development` has properly "mixed" in those css styles:

![tailwind-mix.png](/images/tailwind-mix.png)

You can see the `mix` command being exectued. Once complete, access to the red color is available in your compiled css. By using the `npm run watch` command, you'll see that the `development` script is executed upon changes to your codebase. 

