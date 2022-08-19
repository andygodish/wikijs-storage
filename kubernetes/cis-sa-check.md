---
title: Kubectl Patch Bindings - CIS Benchmark 5.1.5
description: Kubectl one-liner used to removed the default service account from all clusterrole and role bindings.
published: true
date: 2022-08-18T04:10:13.080Z
tags: kubernetes, cis, kubectl
editor: markdown
dateCreated: 2022-08-17T03:13:18.918Z
---

# CIS Benchmark 5.1.5

Work on a particular ticket revealed that [this cis benchmark](https://rancher.com/docs/rancher/v2.6/en/security/hardening-guides/rke-1.6-benchmark-2.6/#5-1-5-ensure-that-default-service-accounts-are-not-actively-used-automated) remediation only remediates the first half of the script used to check for the vulnerability. 

## Service Account Check 

The second check in the check_sa.sh script would be remediated by patching clusterrole and role binding resources so that the default service account is removed. 

```
for ns in $(kubectl get ns --no-headers -o custom-columns=":metadata.name")
do
    for result in $(kubectl get clusterrolebinding,rolebinding -n $ns -o json \
      | jq -r '.items[] \
      | select((.subjects[].kind=="ServiceAccount" and .subjects[].name=="default") or \
        (.subjects[].kind=="Group" and .subjects[].name=="system:serviceaccounts"))' \
      | jq -r '"\(.roleRef.kind),\(.roleRef.name)"')
    do
        read kind name <<<$(IFS=","; echo $result)
        resource_count=$(kubectl get $kind $name -n $ns -o json | jq -r '.rules[] | select(.resources[] != "podsecuritypolicies")' | wc -l)
        if [[ ${resource_count} -gt 0 ]]; then
            echo "false"
            exit
        fi
    done
done
```

1. Loops through namespaces in the cluster
2. For each namespace, get all clusterrole and role bindings where either of the following are true:
    - subjects[].kind == ServiceAccount AND subjects[].name == default
    - subject.[].kind == Group AND subjects[].name == system:serviceaccounts

3. The kind and name are piped out using jq on the list of returned json
4. The kind (k8s resource type) and its name are parsed from the comma-separated output and used to make the following kubectl command using scoped variables, providing a count of resources that are "out of compliance":
     
    - `k get $kind $name -n $ns -o json | jq -r '.rules[]' | select(.resources[] != "podsecuritypolicies")' | wc -l`
    	- piped a json query against the rules, where kind in this case is either a role or clusterrole
    	- piped select filtering out podsecuritypolicy resources
    	- piped count of lines (`-l`) in the output 

5. If the result of the `wc -l` is greater than zero, the script exits after echo false, indicating a failure to meet the benchmark

## Remove Subject from Binding


