---
title: virtualenv
description: Creating isolated project spaces in python. 
published: true
date: 2023-02-23T03:05:01.233Z
tags: python, virtualenv
editor: markdown
dateCreated: 2023-02-23T03:05:01.233Z
---

# virtualenv

One of the most popular tools to create isolated project environments in Python is virtualenv. It allows you to create a sandboxed Python environment where you can install the necessary packages for your project without affecting your system's Python installation.

To install virtualenv, you can use pip:

```
pip install virtualenv
```
Once you have virtualenv installed, you can create a new environment by running the following command in your project directory:

```
virtualenv myenv
```

This will create a new directory called myenv in your project directory, which contains a self-contained Python environment. You can activate this environment by running:

```
source myenv/bin/activate
```
After activating the environment, any packages you install using pip will be installed only in the myenv directory and won't affect your system's Python installation. You can deactivate the environment by running:

```
deactivate
```

There are also other popular tools for creating isolated project environments in Python, such as conda and pipenv, which provide additional features such as dependency management and environment configuration.