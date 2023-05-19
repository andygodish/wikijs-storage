---
title: Upgrading RHOL Operator
description: Upgrading the RHOL Operator
published: true
date: 2023-05-19T17:40:23.455Z
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

![operator-settings.png](/operator-settings.png)

You'll see the existing operator installation error out and eventually clear before the new operator instance is installed. 

## Configuring the Operator

### Run the Operator on Specific Nodes

The operator itself should be run on an infrastructure node (not sure if an infa node is openshift specific or if it is synonymous with controlplane/master node). Update the yaml for the operator resource itself and add the appropirate `nodeSelector` and `tolerations` as indicated by your cluster. Those yaml configurations should be placed at,

- `spec.install.spec.deployments[].cluster-logging-operator.spec.template.spec.nodeselector`
- `spec.install.spec.deployments[].cluster-logging-operator.spec.template.spec.tolerations`

### Create OpenShift Logging Instance

In the openshift portal:

`Installed Operators > Red Hat OpenShift Logging > Cluster Logging`

From this page, create a new one.

This is where you can definie configurations specific to Splunk. See OpenShift documentation on forwarder configurations specific to your logging tool. In my case, we wanted a configuration that did not utilize ElasticSearch. The end result is the creation of a CR called `ClusterLogging`.

- [More information about the OpenShift Custom Resources](https://docs.openshift.com/container-platform/4.10/logging/cluster-logging.html#cluster-logging-about_cluster-logging)

### Creating the Forwarder Pods

#### Create a Splunk HEC

This get applied as a secret in the openshift-loggin namespace. This can be created in Spluk by navigating to:

`Settings (NavBar) > Data Inputs > HTTP Events Collector (Local Inputs)`

From here, you can define a new token. In my case, I want to set the "selected index" to just use "openshift." There are 3 clusters in our infrastructure, all of which route logs to this index. The logs from different clusters are separated by the "Source" field. This can be set when creating the token.

In the openshift-logging namespace, create a secret called `splunk-token` with a single field called, `hecToken`,

```
oc -n openshift-logging create secret generic splunk-secret --from-literal hecToken=<HEC_Token>
```

You can then reference this secret in the spec.outputs of the ClusterLogForwarder CR ike so:

```
apiVersion: logging.openshift.io/v1
kind: ClusterLogForwarder
metadata:
  managedFields:
  name: instance
  namespace: openshift-logging
spec:
  outputs:
    - name: splunk-receiver
      secret:
        name: splunk-token
      type: splunk
      url: 'https://...
```

#### Additional Configuration Settings

The ClusterLogForwarder will also need to be configured to only take `application` logs as inputRefs. This, combined with the ClusterLogging CR only being configured to run on worker nodes will prevent "infrastructure" logs from inundating our Splunk cloud instance with way more information that we need. Other tools are used in our organization for infrastructure-specific logging.

```
apiVersion: logging.openshift.io/v1
kind: ClusterLogForwarder
metadata:
  managedFields:
  name: instance
  namespace: openshift-logging
spec:
  outputs:
    ...
  pipelines:
    - inputRefs:
        - application
```

### Configure HAProxy to Forward via Syslog

Configure the OpenShift default HAProxy router to send access logs to the Splunk forwarders via syslog by editing the OpenShift ingresscontroller's `httpLogFormat` field: 

```
spec:
  ...
  logging:
    access:
      destination:
        syslog:
          address: 10.34.6.40
          port: 514
        type: Syslog
      httpCaptureHeaders:
        ...
      httpLogFormat: '%ci:%cp %ft %b/%s %TR/%Tw/%Tc/%Tr/%Ta %ST %B %CC %CS %tsc %ac/%fc/%bc/%sc/%rc %sq/%bq %hr %hs %HM %HP %{+Q}HQ %HV'
```

## Verify Logs are Being Received in Splunk

Query Splunk by using the index and source defined in the HEC token that was created earlier: 

```
index="<index>" source="<source>"
```

