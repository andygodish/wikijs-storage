---
title: Landing (TAP, cicd)
description: About for andyg.io landing page. Documentation for TAP presentation on CICD processes.
published: true
date: 2024-04-03T14:34:18.160Z
tags: cicd, devops, andyg.io, tap
editor: markdown
dateCreated: 2024-03-23T14:39:09.073Z
---

# andyg.io Landing Page (TAP, cicd presentation)

- [Source Code](https://github.com/andygodish/landing)
- [Terraform Project (production infrastructure)](https://github.com/andygodish/terraform-agio-landing)
- [Landing Page](https://andyg.io)

This project was built as presentation material for the Technical Apprenticeship Program (TAP) demonstration of how CICD pipelines are implemented in our organization.

![landing-cicd.jpg](/images/landing-cicd.jpg)

## GitHub Actions

[GitHub Actions](https://github.com/features/actions) is primarily used to construct CI pipelines. 

### Workflows

> A GitHub Workflow is a configurable automated process composed of one or more jobs that you can set up in your GitHub repository to build, test, package, release, and deploy your project. Workflows are defined in a `YAML` file within the .github/workflows directory of a repository and are triggered by `GitHub events` like push, pull request, or manual invocation. Each workflow can contain a series of steps that run commands, scripts, or actions, which are reusable pieces of code that can perform tasks such as setting up a programming environment or deploying software.