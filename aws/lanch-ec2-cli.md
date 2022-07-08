---
title: Launch EC2 using AWS CLI
description: Guide to launching EC2s using the AWS CLI with the intent of adding additional nodes existing K8s clusters.
published: true
date: 2022-07-08T01:48:28.722Z
tags: kubernetes, aws, aws-cli
editor: markdown
dateCreated: 2022-07-08T01:48:28.722Z
---

# Launching EC2s via AWS CLI

I typically deploy test clusters to AWS using a terraform script that gives me a set number of nodes. While that number can be controlled via a variable in terraform, at times it is easier to just deploy set of nodes on the fly. 

## Prerequisites
Configure your local environment with the proper aws cli variables.

```
export AWS_ACCESS_KEY_ID=**********
export AWS_SECRET_ACCESS_KEY=******
export AWS_DEFAULT_REGION=us-east-1
```

[AWS Documentation](https://docs.aws.amazon.com/cli/latest/userguide/cli-services-ec2-instances.html)

The following will disect this command given in the AWS documentation: 

```
aws ec2 run-instances --image-id ami-xxxxxxxx --count 1 --instance-type t2.micro --key-name MyKeyPair --security-group-ids sg-903004f8 --subnet-id subnet-6e7f829e
```

And use other parts of the cli to gather the following information: 

- subnet-id
- security-group-ids
- key-name
- image-id

## Describe-Instances

All of the information we need can be found with the `ec2 describe-instances` endpoint along with the addition of filters. In most of my AWS lab environments, I use the tag *Owner: andyg*. 

Use the `--filters` option to search for instances with this tag: 

```
--filters "Name=tag:Owner,Values=andyg"
```

Query for these results with the following `--query` options:

```
--query 'Reservations[*].Instances[*].[ImageId,InstanceType,KeyName,SubnetId,SecurityGroups[].GroupId]'
```

All together: 

```
aws ec2 describe-instances \
--filters "Name=tag:Owner,Values=andyg" \
--query 'Reservations[*].Instances[*].[ImageId,InstanceType,KeyName,SubnetId,SecurityGroups[].GroupId]' \

```

## Create Instances

With that info the `ec2 run-instances` command can be run to quickly stand up instances that fit the environment where your exisiting cluster nodes are deployed. 

```
aws ec2 run-instances \
--image-id ami-bbba86da \
--count 3 \
--instance-type t2.xlarge \
--key-name andyg-keypair-goV9V8 \
--security-group-ids sg-0c5dfed0214263487 \
--subnet-id subnet-093d1fa49a2b408b2 \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Owner,Value=andyg}]'
```

- **TODO: Update to include public IP address**

## Joining Nodes to Your Existing Cluster

### RKE

RKE is probably the simplest K8s distribution to join nodes. In the case of a typical lab environment that I run, my nodes are generally public facing. In this case, just grab the `PublicIpAddress` of each new instance and add them to your cluster.yml file. 

## Delete Instances

Since these are not controlled by Terraform, you
