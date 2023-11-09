---
title: Debugging in Ansible
description: Debugging in Ansible
published: true
date: 2023-11-09T15:54:21.761Z
tags: ansible, debug
editor: markdown
dateCreated: 2023-11-09T15:54:21.761Z
---

# Debugging in Ansible

## Example

```
- name: "[install] Check the binary locations for correct ~/.zshrc rendering"
  ansible.builtin.command: "which {{ item }}"
  loop:
    - zsh
    - terraform
  register: ohmyzsh_binaries
  changed_when: false
  failed_when: false

- name: Debug output of binary paths
  ansible.builtin.debug:
    msg: "Path to {{ item.item }} is {{ item.stdout }}"
  loop: "{{ ohmyzsh_binaries.results }}"
```

### {{ item }}

In Ansible, `{{ item }}` is a special keyword used within a loop to reference the current item being processed. When you use loop or the shorthand with_<lookup> in a task, Ansible implicitly creates a loop variable named item that contains the current item from the list that you are looping over.
  
*If you want to change the name of the loop variable from item to something else, you can use the loop_control directive with the loop_var option. However, you cannot just change {{ item }} to a random variable name without informing Ansible of this change, as item is the default loop variable name that Ansible expects.*
  
```
- name: Debug output of binary paths
  ansible.builtin.debug:
    msg: "Path to {{ my_var.item }} is {{ my_var.stdout }}"
  loop: "{{ ohmyzsh_binaries.results }}"
  loop_control:
    loop_var: my_var  
```

### register:
  
When you use the register keyword in an Ansible task that is looped over multiple items, Ansible stores the results in a list under the registered variable. Each item in the list is a dictionary containing the output and other details for the corresponding loop iteration.
  
When looping over a list of "items," ansible stores the results in a dictionary under the list of `results.` 

`register: ohmyzsh_binaries` ---> `loop: "{{ ohmyzsh_binaries.results }}"`
  
Each "result" is a dictionary whose values can be further accessed in the `msg` field of the *debug module.*
