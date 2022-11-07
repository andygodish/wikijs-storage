---
title: Output Variables
description: Explanation and examples of how to use output variables on your projects.
published: true
date: 2022-11-07T23:36:29.644Z
tags: octopus
editor: markdown
dateCreated: 2022-11-02T20:34:30.513Z
---

# Octopus Output Variables

Output variables can be defined anywhere you can write a script in either bash or PS. These are different from project variables, and these functions/cmdlets should not be used to attempt to modify a project variable. 

## Setting and Accessing an Output Variable

This example involved writing a powershell script in one step to dynamically set a variable. 

The first step is a PS script that create a RoleRef varable with a default value of `admin`. Using an Octopus `system variable`, it reads in the name of the environment. If that env name includes `uat-` or `qa-`, the RoleRef is reassigned the value of `basic-user`. Lets say name of this step is `RR Create Output Variable`.

```
# PS
Set-OctopusVariable -name "RoleRef" -value "admin"

$Environment="#{Octopus.Environment.Name}"
$EnvTypes=@("uat-", "qa-")

$Counter=0

foreach ($Type in $EnvTypes) {
	if ($Environment.Contains($Type)) {
		Write-Output "roleRef in RoleBinding will be set to basic-user"
    	Set-OctopusVariable -name "RoleRef" -value "basic-user"
        break
    }
    $Counter++
}

if ($Counter -eq $EnvTypes.Length) {
	Write-Output "roleRef in RoleBinding will be set to basic-user"
}

```

A subsequent step involves accessing this output varialbe in a yaml file. The format for doing so looks like this: 

```
# YAML file
---
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: #{Octopus.Action[RR Create Output Variable].Output.RoleRef}
---
```

Note the clunky reference to the name of the step that produces the output variable. This is not made very clear in the Octopus documentation and took some playing around to figure it out. 