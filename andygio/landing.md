---
title: Landing (TAP, cicd)
description: About for andyg.io landing page. Documentation for TAP presentation on CICD processes.
published: true
date: 2024-04-04T16:30:42.665Z
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

![ci-pipeline.png](/images/ci-pipeline.png)

- [Source Code] (https://github.com/andygodish/landing/blob/main/.github/workflows/pipeline.yaml)

The CI portion of the pipeline is kicked off when code is either pushed or mergered from a pull request into the main branch as definined in the `on:` field of the workflow yaml. The workflow consists of the following `jobs`:

#### test

Nodejs is required to execute the `npm test` command outlined in the package.json file. `Jest` is the testing library used for unit testing as definied by the `devDependencies` in the package.json file.

#### version

A tool, originally written and designed for versioning npm packages, rewritten in Golang, called Semantic Release is used to programatically create releases and semantic version tags in the GitHub repository. 

- [Semantic Release (original)](https://semantic-release.gitbook.io/semantic-release)
- [Go Semantic Release](https://github.com/go-semantic-release/action)
- [Actions Marketplace](https://github.com/marketplace/actions/go-semantic-release)

It scans commit messages between releases and determines the proper version change using [Contentional Commits](https://www.conventionalcommits.org/en/v1.0.0/).

#### build

This is a docker image build of the `dockerfile` in the root directory. It tags the image with the version specified by the `version` job. 

## CD

This app is autmatically deployed to a Kubernetes cluster running in my homelab. [FluxCD](https://fluxcd.io/) is used 


