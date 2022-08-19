---
title: Querying Elastic Load Balancers
description: Quick hitter on how to use the AWS CLI to query ELBs.
published: true
date: 2022-08-18T04:09:38.632Z
tags: aws
editor: markdown
dateCreated: 2022-08-17T03:12:30.089Z
---

# Querying ELBs with the AWS CLI

## Prerequisites

Configure your local environment with the proper aws cli variables. 

```
export AWS_ACCESS_KEY_ID=**********
export AWS_SECRET_ACCESS_KEY=******	
export AWS_DEFAULT_REGION=us-east-1
```

## Command

Example scenario: 

Using my RKE2 bootstrap script requires the DNS name of the control plane loadbalancer, typically created previously using Terraform. The following command will give me a list of the DNS names associated with the ELBs in the configured region. 

```
aws elb describe-load-balancers --query 'LoadBalancerDescriptions[*].DNSName'
```