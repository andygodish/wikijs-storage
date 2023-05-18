---
title: Upgrading RHOL Operator
description: Upgrading the RHOL Operator
published: true
date: 2023-05-18T22:08:13.677Z
tags: kubernetes, openshift, logging
editor: markdown
dateCreated: 2023-05-18T22:08:13.677Z
---

# Upgrading RHOL Operator

My organization using the Red Hat OpenShift Logging Operator to forward logs to our Splunk Cloud subscription. 

### Splunk Forwarders

Earlier versions of the Red Hat OpenShift Logging Operator required the use of a fluentd forwarder in addition to the log collector. Both of these were deployed as pods in our clusters. However, the forwarders were installed and managed via a helm chart external to the OpenShift operator. This helm chart release can be deleted prior to upgrading the Operator. 

```
helm delete --namespace openshift-logging openshift-logforwarding-splunk
```

## Upgrade the Operator

At the time of this writing, 5.3.14 of the operator is in use. The target upgrade is the stable channel (5.6). Update the channel make sure your update approval process is set to manual. This will require a manual change to the operator manifest when performing future upgrades. Whereas setting it to automatic is analogous to setting the version tag to `latest` in a k8s manifest. 

You'll see the existing operator installation error out and eventually clear before the new operator instance is installed. 

## Configuring the Operator

### Run the Operator on Specific Nodes

The operator itself should be run on an infrastructure node (not sure if an infa node is openshift specific or if it is synonymous with controlplane/master node). Update the yaml for the operator resource itself and add the appropirate `nodeSelector` and `tolerations` as indicated by your cluster. 

### Create OpenShift Logging Instance

In the openshift portal:

`Installed Operators > Red Hat OpenShift Logging > Cluster Logging`

From this page, create a new one.

This is where you can definie configurations specific to Splunk. See OpenShift documentation on forwarder configurations specific to your logging tool. In my case, we wanted a configuration that did not utilize ElasticSearch. 

### Creating the Forwarder Pods

#### Create a Splunk HEC

This get applied as a secret in the openshift-loggin namespace




