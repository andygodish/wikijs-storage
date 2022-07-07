---
title: AWS CLI - Tag an EC2
description: Filter and tag a set of existing EC2 instances
published: true
date: 2022-07-07T23:17:19.315Z
tags: aws, aws-cli, ec2
editor: markdown
dateCreated: 2022-06-30T00:57:55.210Z
---

# Taggin EC2s with AWS CLI

## Prerequisites
Configure your local environment with the proper aws cli variables.
```
export AWS_ACCESS_KEY_ID=**********
export AWS_SECRET_ACCESS_KEY=******
export AWS_DEFAULT_REGION=us-east-1
```

## Filter for Target EC2s and Query for Needed Data

I often tag my AWS resources with the key-value pair, Owner: andyg. I can filter these instances using `ec2 describe-instances` with that tag set as a `--filters`.

```
aws ec2 describe-instances --filters "Name=tag:Owner,Values=andyg" --query 'Reservations[*].Instances[*].InstanceId'
```

Returned results will be a list of instanceIds that can be modified.

### Querying Multiple Fields

[Reference](https://serverfault.com/questions/669350/aws-cli-command-line-how-to-use-query-to-output-multiple-source-lines)

Note the brackets in the last declaration.

--query 'Reservations[].Instances[]`.[InstanceId,ImageId]`'

## Add a Tag

[AWS docs](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-tags.html)

```
aws ec2 create-tags --resources <resourceId-1> <resourceId-2> --tags Key=KeepRunning,Value=true
```

Where `resourceId-n` corresponds to the output returned in the previous command. If the tag already exists on the resource, the command will be ignored. 