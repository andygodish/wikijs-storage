---
title: Iptables Basics
description: Just like the title says.
published: true
date: 2022-08-18T04:13:39.940Z
tags: linux, iptables
editor: markdown
dateCreated: 2022-08-17T03:18:43.635Z
---

# Iptables Basics	

The main internface into the linux kernel **netfilter** system (linux packet filtering). So it is esentially a firewall.

Iptables is a set of rules contained within chains, contained within tables. 

## Three Tables

- Mangle 
- Nat 
- Filter (deftault table)

*To list all chains within the default table (Filter), run the following:*

```
iptables -L
```
- Default table will be outputted
- RHEL systems will have rules by default. 

*More verbose output:*

```
iptables -L -n -v --line-numbers
```
where,
- `-n` numbers instead of names (0.0.0.0/0 vs anywhere)
- `-v` verbose output
- `--line-numbers` shows the number in the chain

## Three Default Chains (Filter Table)

- Input 
- Forward 
- Output 

Kubernetes (the CNI specifically) adds several additional chaines. By default, RHEL systems supposedly come with preconfigured rules, this does not appear to be the case in my preconfigured centos-stream proxmox clone. 

By default, you should see a policy ACCEPT for each Chain. This indicates the default action that should take place if there is not a matching rule in the chain.

Example: by default, all incoming traffic is allowed (INPUT chain), because there are no rules and a fallback policy of ACCEPT.

```
Chain INPUT (policy ACCEPT)
```

## Policies

- ACCEPT
- REJECT - will send an ICMP rejection back to sender
- DROP - will not

## Examples

*Add a rule to allow all incoming and outgoing traffic, and change the default policy to drop.*

sudo iptables -A CHAIN -j POLICY
sudo iptables -P CHAIN POLICY

```
# All trafic 
sudo iptables -A INPUT -j ACCEPT
sudo iptables -A OUTPUT -j ACCEPT

sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT DROP
```
where,
- `-A` append a rule to a chain
- `-j` target (or jump chain) 
- `-P` policy

---

*Allow all loopback traffic*

sudo iptables -A CHAIN -j POLICY -TRAFFIC-DIRECTION INTERFACE

```
sudo iptables -A INPUT -j ACCEPT -i lo 
sudo iptables -A OUTPUT -j ACCEPT -o lo 
```
where,
- `-i lo` incoming traffic on the loopback interface
- `-o lo` outgoing traffic


## Comments

Add a comment to a rule with the `comment` module using the `--comment` parameter followed immediately by your comment in single quotes. 

```
sudo iptables -A INPUT -j ACCEPT -p tcp --dport 22 -m comment --comment 'allow ssh from all'
```
