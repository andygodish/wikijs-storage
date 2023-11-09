---
title: Template Module
description: How to use the built in Ansible Template Module
published: true
date: 2023-11-09T16:20:38.093Z
tags: ansible, template, ansible-module
editor: markdown
dateCreated: 2023-11-09T16:20:38.093Z
---

# Template Module

`ansible.builtin.template`: The template module is used to process a file through the `Jinja2 templating engine`, creating a file on the target machine. This is typically used to manage configuration files with variables that can be replaced with actual values during the execution of the playbook.

```
- name: "[install] Setup zprofile for zsh"
  ansible.builtin.template:
    src: zprofile.j2
    dest: "{{ ohmyzsh_zprofile_dir }}/zprofile"
    mode: 0644
    owner: root
    group: root
  become: true
  when: "'Darwin' not in ansible_os_family"
```

By default, Ansible looks for template files in the templates directory relative to the playbook. This is part of Ansible's standard convention. When you specify a source template file like *zprofile.j2*, Ansible will automatically search for this file in the templates directory unless you provide an absolute path or a path relative to another recognized directory, like roles/your_role/templates/ if the task is part of a role.

```
# tree
roles
├── ohmyzsh
		├── templates
        ├── zprofile.j2
```

If you want to store your templates elsewhere, you would have to specify the path to the template file relative to your playbook or as an absolute path on your control machine.

