---
title: SSH Configuration
description: How-to guid on configuring ssh access using a key for authentication.
published: true
date: 2022-08-18T04:11:30.319Z
tags: linux, ssh
editor: markdown
dateCreated: 2022-08-17T03:15:08.905Z
---

# SSH Configuration

[Reference Guide](https://www.thegeekdiary.com/how-to-setup-ssh-keys-for-passwordless-ssh-login-on-centos-rhel/)

Because I always forget, here are the steps to configure ssh using keys instead of a password.

## Generate Keys on the Local Host

This most likely already exists, but just in case. This will create a key for the signed in user. The defaults work great. 

```
ssh-keygen
```

## Copy Local Public Key to the Remote Host

This will add the public key of your local host to the authorized_keys file located in your .ssh directory.

```
ssh-copy-id -i id_rsa.pub user@host
```

## Disable SSH Password Requirement

[Reference Guide](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-centos-8)

Edit the sshd_config file to include the following two lines. Note, by default I think publickeyaccess is required. 

```
sudo vi /etc/ssh/sshd_config
---
PasswordAuthentication no
PubkeyAuthentication yes
```

Restart the sshd service

```
sudo systemctl restart sshd
```

Done. Test it out. 
