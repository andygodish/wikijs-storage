---
title: Ansible Installation
description: Primer on getting started with Ansible. 
published: true
date: 2022-08-31T20:58:46.072Z
tags: ansible
editor: markdown
dateCreated: 2022-08-31T20:44:15.451Z
---

# Ansible Installation	

- Following the [Ansible docs](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#id6).

I'm attempting to use my Windows WSL 2 environment as my control node. According to the documentation, this should be a viable option. You'll need Python 3.8 at a minimum, 3.10 shipped with my Ubuntu 22.04 iso. In addition, you'll need pip. I found that Ubuntu 22.04 did not come with it standard. The Ansible documentation recommends the following to install the latest pip directly from the Python Packaging Authority:

```
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py --user
```

- I am not exactly sure what the `--user` flag is doing in these commands

## Install Using pip

```
python3 -m pip install --user ansible
```

### command not found: ansible

- [Github Comment](https://github.com/ansible/ansible/issues/67259#issuecomment-589269174)

> Our pip installation indicates using pip install --user ansible which will install to ~/.local, but you must add ~/.local/bin to your $PATH env var.

Add this directory to your path variable: 

```
export PATH=$PATH:~/.local/bin
```

### Auto-completion

I'm using zsh in my environment. It's worth noting that the docs state support is limited for zsh. 

```
python3 -m pip install --user argcomplete

activate-global-python-argcomplete
```

Zsh specific:
```
autoload -U bashcompinit
bashcompinit
```

Example `~/.zshrc` file:

```
autoload bashcompinit
bashcompinit

eval $(register-python-argcomplete ansible)
eval $(register-python-argcomplete ansible-config)
eval $(register-python-argcomplete ansible-console)
eval $(register-python-argcomplete ansible-doc)
eval $(register-python-argcomplete ansible-galaxy)
eval $(register-python-argcomplete ansible-inventory)
eval $(register-python-argcomplete ansible-playbook)
eval $(register-python-argcomplete ansible-pull)
eval $(register-python-argcomplete ansible-vault)
```