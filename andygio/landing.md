---
title: Landing (TAP, cicd)
description: About for andyg.io landing page. Documentation for TAP presentation on CICD processes.
published: true
date: 2024-04-03T17:15:42.528Z
tags: cicd, devops, andyg.io, tap
editor: markdown
dateCreated: 2024-03-23T14:39:09.073Z
---

# andyg.io Landing Page (TAP, cicd presentation)

- [Source Code](https://github.com/andygodish/landing)
- [Terraform Project (production infrastructure)](https://github.com/andygodish/terraform-agio-landing)
- [Landing Page](https://andyg.io)

This project was built as presentation material for the Technical Apprenticeship Program (TAP) demonstration of how CICD pipelines are implemented in our organization.

## Overview

![landing-cicd.jpg](/images/landing-cicd.jpg)

## CI

The pipeline is initiated by a push to the main branch.

### Compilation

At a minimum, a CI pipeline should verify that the code can be successully integrated into the existing production code. This is done by compiling the code using the tools of the associated programming language. 

Since this project is only comprised of html, css, and js files, there is not "compilation" step. Rather the build step is a build of a container image. The package.json is used for node packages involved with unit testing. 

### Unit Testing

Another staple of a CI pipline is the automated execution of unit tests. Before integrating new code, verification that the code has not broken production code protected by previously written unit tests. 

### Pipeline

- [Source Code] (https://github.com/andygodish/landing/blob/main/.github/workflows/pipeline.yaml)

The CI portion of the pipeline is kicked off when code is either pushed or mergered from a pull request into the main branch as definined in the `on:` field of the workflow yaml. 




