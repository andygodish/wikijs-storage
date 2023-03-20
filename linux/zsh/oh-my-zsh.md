---
title: OhMyZsh
description: Configuration and plugin information for my flavor of zsh
published: true
date: 2023-03-20T14:28:52.026Z
tags: linux, zsh
editor: markdown
dateCreated: 2022-12-03T06:32:11.701Z
---

# OhMyZSH

Configurations for oh my zsh are stored at your account's home directory in the `.zshrc` file.

Installation first requires that zsh is installed. `apt install zsh`.

During the installation process, you are asked if you want to make zsh your default shell. Double check how this is implemented for your ansible playbook.

## Plugins

Add code for your plugin into the plugins directory at your $ZSH_CUSTOM path (by default `~/.oh-my-zsh/custom/plugins`)


- [autosuggestions](https://github.com/zsh-users/zsh-autosuggestions/blob/master/INSTALL.md)

## Store Aliases

Aliases can be added to your users .zshrc file. I usually throw mine at the end in a designated section. 

```
alias k=kubectl
```