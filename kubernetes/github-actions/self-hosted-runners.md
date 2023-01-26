---
title: Managing Self Hosted Github Runners
description: Tidbits about managing self hosted github actions runners. 
published: true
date: 2023-01-26T21:32:34.632Z
tags: github, cicd, github actions
editor: markdown
dateCreated: 2023-01-26T05:30:10.862Z
---

# Managing Self Hosted Github Runners

I wrote an unanswered issue in the upstream repository for the openshift maintained runner images summarizing an issue that I am seeing when these runner pods run workloads for an extended period of time. More information can be assertained by reading the issue itself [here](https://github.com/redhat-actions/openshift-actions-runners/issues/23). 

There are two primary issues that I encountered, one is that the use of the redhat public actions break the configurations specific to the OpenShift cluster I use at work, and the second appears to be some kind of caching that is taking place in the `.local` directory on each pod's filesystem. I haven't confirmed that this is in fact the issue, but troubshooting efforts that involve wiping that directory, or the overaching pod, result in a functioning runner. 

To get around this issue, my plan is to utilize the `base` image found in the same OpenShift repo mentioned above as an intermediary runner that would simply spin up short lasting runners with more application specific tools needed by a development team. 

For example, say you have a workflow that just needs to build and push an image to a registry. To avoid the supposed caching problem resulting from a long-running runner-pod that builds and stores images layers, I have a simpler pod that utilizes `kubectl` to first spin up the required runners, then, once those subsequent runners have completed their jobs, the simple runner can then scale down the deployment associated with the required runner. This approach assumes runner deployments sitting idle at 0 replicas.

## Deploying the Base Runner

The following example will be deployed at the repository level. It's important to note that organizational and enterprise-level runners require slightsle different configurations.

### Env Variables

```
export RELEASE_NAME="base-runner"                        
export GITHUB_OWNER=andygodish
export GITHUB_PAT=XXXXXXXXXXXX
export GITHUB_REPO=vue-quickstart
```

Where `RELEASE_NAME` is the future name of you helm release. 

### Install with Helm

- [Upstream Helm Chart](https://github.com/redhat-actions/openshift-actions-runner-chart)
- [Modified Helm Chart](https://github.com/vr-infrastructure/openshift-gh-runners-chart)


#### Service Account & Secret

Set your helm chart values to utilize a pre-created `serviceaccount` and container registry secret. By default, the helm chart expects a service account named `runner`. 

```
kubectl create serviceaccount runner
```

I'm using quay.io to pull in the image registry. They provide a copy and paste option for yaml associated with a pull secret. It'll look something like this once generated:

```
apiVersion: v1
kind: Secret
metadata:
  name: quay
data:
  .dockerconfigjson: XXXXX
type: kubernetes.io/dockerconfigjson
```

Create the secret from the yaml file and add the secret name to you helm values under `imagePullSecrets`

---

Assuming you already have a Kubernetes cluster to work with,

```
helm install base-runner ./ \    
  --set-string githubOwner=$GITHUB_OWNER \
  --set-string githubPat=$GITHUB_PAT \
  --set-string githubRepository=$GITHUB_REPO \
  --set runnerLabels="{helm, base, dev}" \
  --set runnerImage=quay.io/andygodish/gh-runners-base \
  --set runnerTag=v1.0.0
```

## Deploying the Buildah Runner

Same command as above, just change the image to `quay.io/andygodish/gh-runners-builhah` and the number of replicas to `0`.

```
helm install buildah-runner ./ \
  --set-string githubOwner=$GITHUB_OWNER \
  --set-string githubPat=$GITHUB_PAT \
  --set-string githubRepository=$GITHUB_REPO \
  --set runnerLabels="{helm, base, dev}" \
  --set runnerImage=quay.io/andygodish/gh-runners-buildah \
  --set runnerTag=v1.0.0 \
  --set replicas=0
```