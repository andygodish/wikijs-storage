---
title: Longhorn Installation Requirements
description: How to guide on installing the prerequisites required for Longhorn.
published: true
date: 2022-08-18T04:11:47.515Z
tags: kubernetes, longhorn
editor: markdown
dateCreated: 2022-08-17T03:15:33.720Z
---

# Longhorn Requirements

[Longhorn Docs](https://longhorn.io/docs/1.3.0/deploy/install/#installation-requirements)

The follow requirements must be met on each node in your cluster: 
- A container runtime compatible with Kubernetes (Docker v1.13+, containerd v1.3.7+, etc.)
- K8s v1.18+
- open-iscsi is installed, and the iscsid daemon is running on all the nodes
- if RWX support is required 
	- NFSv4 
- The host filesystem supports the file extents feature to store the data
- The following must be installed:
	- bash
	- curl
	- findmnt
	- grep
	- awk
	- blkid
	- lsblk
  
## Verification Script

Run this on your local machine, not on each node directly. Requires `jq` and `kubectl` to be installed; kubectl should be able to reach your cluster. 
```
curl -sSfL https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/scripts/environment_check.sh | bash
```

### Findings
- Centos lab environment only required that I install and enable iscsi, all other requirements were met out of the box
- Unable to use a TTY - input is not a terminal or the right kind of file
	- outputted when running the verification script
  - [Stackoverflow Reference](https://stackoverflow.com/questions/60826194/kubectl-exec-fails-with-the-error-unable-to-use-a-tty-input-is-not-a-terminal)
  	- When the flags -it are used with kubectl exec, it enables the TTY interactive mode. Given the error that you mentioned, it seems that some services dont allocate a TTY.

## Installing open-iscsi

[Longhorn Docs](https://longhorn.io/docs/1.3.0/deploy/install/#installing-open-iscsi)

Easiest way is to just use the iscsi-installer provided by LH: 

```
kubectl apply -f https://raw.githubusercontent.com/longhorn/longhorn/v1.3.0/deploy/prerequisite/longhorn-iscsi-installation.yaml

```