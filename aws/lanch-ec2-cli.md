---
title: Launch EC2 using AWS CLI
description: Guide to launching EC2s using the AWS CLI with the intent of adding additional nodes existing K8s clusters.
published: true
date: 2022-08-18T04:09:47.248Z
tags: aws, kubernetes, aws-cli
editor: markdown
dateCreated: 2022-08-17T03:12:41.961Z
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
[ec2 run-instances --options](https://docs.aws.amazon.com/cli/latest/reference/ec2/run-instances.html)

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

If needed, you can filter by running instances: 

```
--filters "Name=instance-state-name,Values=running"
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
--associate-public-ip-address \
--image-id ami-xxxxxxxx \
--count 3 \
--instance-type t2.xlarge \
--key-name andyg-keypair-xxxxxx \
--security-group-ids sg-xxxxxxxxxxxxxx \
--subnet-id subnet-xxxxxxxxxxxxxx \
--tag-specifications 'ResourceType=instance,Tags=[{Key=Owner,Value=andyg}]'
```

- **TODO: Update to include public IP address**

## Joining Nodes to Your Existing Cluster

### RKE

RKE is probably the simplest K8s distribution to join these new nodes. In the case of a typical lab environment that I run, my nodes are generally public facing. Just grab the `PublicIpAddress` of each new instance and add them to your cluster.yml file. 

```
aws ec2 describe-instances --filters "Name=tag:Owner,Values=andyg" "Name=instance-state-name,Values=running" --query 'Reservations[].Instances[].[LaunchTime,PublicIpAddress]'
```


```
# cluster.yml

nodes:
    - address: 192.168.0.101
      user: centos
      role:
        - controlplane
        - etcd
        - worker
      ssh_key_path: /path/to/key
    - address: 192.168.0.116
      user: centos
      role:
        - worker
      ssh_key_path: /path/to/key
    - address: 192.168.0.118          ##### Start New Nodes
      user: centos
      role:
        - worker
      ssh_key_path: /path/to/key
    - address: 192.168.0.113
      user: centos
      role:
        - worker
      ssh_key_path: /path/to/key
    - address: 192.168.0.186
      user: centos
      role:
        - worker
      ssh_key_path: /path/to/key
```

#### Install Docker on New Nodes

- **TODO: Create a reference to your ansible playbook**

## Delete Instances

Since these are not controlled by Terraform, make sure these are deleted after use. 

[AWS Documentataion](https://awscli.amazonaws.com/v2/documentation/api/latest/reference/ec2/terminate-instances.html).

```
aws ec2 terminate-instances --instance-ids <i-instanceId1> <i-instanceId2> 
```
