---
title: Publishing a Release
description: Notes on publishing releases to github
published: true
date: 2022-10-09T05:31:23.218Z
tags: github, cicd
editor: markdown
dateCreated: 2022-10-09T04:56:15.874Z
---

# Publish a Release

- [Python Example](https://github.com/andygodish/content-github-actions-deep-dive-lesson/tree/lab)

You will probably want to your build step to follow some sort of linting step that won't require a large package download to complete. With this in mind, you are introducing a dependency to your build `step`. You can insure that a step won't kick off until a specified step is completed by using the `job.needs` in your workflow yaml:

```
job: build
  needs: lint     ***
  steps:
  ...
```

## Publishing a Release

Your publish job will not need to checkout the code again. This is accomplished at the workflow level.

- actions/create-release@v1
	- Requires that you set an env (GITHUB_TOKEN) bc it needs to interact with the github api
  		- This can be pulled from the "run" itself
  
  	```
      steps: 
      - name: create release
        uses: actions/create-release@v1
        env: 
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
    ```

Next, follow the documentation for this particular community action to identify the needed variables:

- tag_name
- release_name
- body
- draft
- prerelease

Variables you can use from github - this becomes important for things like versioning:

- github.run_number
- github.sha

## Release Assets

There are situations where you would want to add a release asset to your release. For example, everything outlined in the previous steps only built out archives of the source code itself. You may want to package all of your application dependencies (pip install) into a single package for users to download. 

In a previous job, a build artifact will likely have already been built. This "build artifact" will have been archived or uploaded (probably more to this) so that it can be referenced in subsequent steps. 

- This may be accomplished with `actions/upload-artifact@v2`.

### Downloading Artifacts

Using the community action, `download-artifact`, you can download your source code archived from an earlier step. You'll need to reference a uniquly named artifact, a la the sha id used to create the zip. 

### Upload the Release Asset

- actions/upload-release-asset@v1
	- This does interact with the github api (secrets.github_token)

You'll need some additional information:

#### Upload Url

This is the endpoint of a specific release where it can place an artifact/asset. This is dynamic, release-to-release, ou will need to utilize `output variables` from previous steps.

This url happens to be an `output` of the create-release step run earlier in the python example. 

##### Output Variables

You can refer to an output in a previous step by using an `id` - you give the step itself a unique id: 

```
step: 
- name: create release
  id: create-release
```

Now, in a dependent job, further down your workflow, you can reference the outputs provided by the actions in the step: 

```
with: 
  upload_url: ${{ steps.create-release.outputs.upload_url }}
```

You've now ensured that your uploading the artifact/asset to the correct release each run. 


#### Asset Path

When you download an artifact, it will be stored in your current working directory. You'll need to point the step to its path, which should look like this in this example (where we are uploading the whole thing):

```
with:
  asset_path: ./${{ github.sha }}.zip
```

#### Asset Name

A name that appears on the release page. This should be descriptive of what's in the file, not specific to the run itself. 

```
with:
  asset_name: source_code_with_libraries.zip
```

#### Application Content Type

Github can be used to manage specific types of assets (ruby gems, container images, etc). In the case of python bundles like this: 

```
with:
  asset_content_type: application/zip
```



