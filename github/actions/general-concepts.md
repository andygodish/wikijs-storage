---
title: Github Actions
description: Basic concepts pertaining to Github Actions
published: true
date: 2022-10-09T00:43:42.392Z
tags: github, git
editor: markdown
dateCreated: 2022-10-09T00:43:42.392Z
---

# Github Actions

I'm working through a github actions deepdive tutorial on A Cloud Guru and using this page as a space to take notes on concepts. 

## Build Job

### Packaging a Lambda Function (Python)

This example involved building a workflow for a simple python program meant to be deployed directly to AWS Lambda. Lambda only needs a program bundle. 

The build job contains 5 `steps`, all of which can be executued on the same runner. 

#### runs-on

Each `job` needs to be assigned to a runner. This example just uses a github hosted runner. I defined `runs-on: ubuntu-latest` in the workflow. This will reach out to github for a runner using the ubuntu-latest image. I believe these are containers used as bases for you to add subsequent package layers to.

#### Defining Actions for Steps to Use

I used several community actions for this job. These can be found at the [github marketplace](https://github.com/marketplace?type=actions). The naming convention of the actions looks like this: 

```
jobs: 
  job: 
    steps: 
    - name
      uses: actions/checkout@v3
```

Where `actions` is the name of the creator, github actions in this case. Note that there are verified creators, much like verified images in docker hub. `checkout` is the name of the community action and `@3` is the version tag used in this example. 

##### Community Actions

- Checkout --- checks out your code (like git pull)
- setup-python --- bootstraps a basic python setup w specific version
- upload-artifact --- archiving artifacts

### Using Variables for Unique Naming

Github has a lot of built-in variables to use within your workflows. In this example, the `github.sha` varaible was used to uniquely name the zip bundle produced during a previous step. 

NOTE: no community action was used to actually zip the repository, instead, the `zip` package was used in a multi-line command (`run`). It is assumed that zip comes packaged with all ubuntu instances.

```
run: |
  cd directory
    zip -r ../${{ github.sha }}.zip .
```

### Uploading Artifacts

I used the upload-artifact community action to take the zip file and upload it as an artifiact. 

When used, you can click on the completed action (actions tab in your repo) and see your artifacts at the bottom of the page. 

- TODO: Find out where Github stores these, and for how long are they cached



