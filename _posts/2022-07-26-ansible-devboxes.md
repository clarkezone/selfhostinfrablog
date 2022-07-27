---
title: Ansible, linux servers, linux devboxes
author: clarkezone
date: 2022-06-27 00:34:00 +0800
categories: [ansible]
tags: [devbox, linux]
---
# Draft - work in progress
## Situation
Over the course of the last few weeks I've found myself repaving the majority of my Windows 11 boxes.  My standard dev setup these days is pretty much exclusively a linux VM either in the form of a WSL session or hosted in a VM running on my main home dev workstation.  I've been casually playing with Ansible for a couple of years now but have never really been "doing it right" having copied and pasted various snippets around.  I now have a sufficiently interesting set of intersecting scenarios that it's time to get rid of all the copy pasting and hard coded credentials and learn how to do it properly.

### Scenarios
Devbox on Linux hosted in the cloud or Hyper-V
Devbox on WSL
Remote monitoring server
Master and worker nodes on self-hosted k3s general purpose clusters

Capabilites
Accounts and lockdown (devboxhosted, monitoring, k3s)
Tailscale (devboxhosted, monitoring, k3s
Devbox setup (devboxhosted, devboxWSL)
k3s (monitoring, gpclusters)

### Questions
1. Monitoring: will min box support k3s and flux
Prototype: use DO script as is, install TS using playbook, users / lockdown as is, k3s from cluster script, try promgrafana stack from cluster, min resources for that

2. What is the repo structure including host provisioning, inventory files, secrets / variables, playbooks, submodules
Prototype: subrepo for inventory and secrets (use strongbox)

```bash
ansible.cfg
provisioning
provisioning\deploydo
inventory\do.ini
inventory\azure.ini
inventory\tailnet.ini
playbooks\
```

3. Passwords / secrets: Ansible Vault, env varaible stored only on builders, via tailscale, checked in using strongbox
Next Step: inventory of all secrets that need extracting to put core capabilities in giht

4. How to stage scenarios to avoid boiling the occean: objectives get 1) prober back 2) all devboxes provisioned 3) not keeping devboxes updated (Hblocked by systemd on WSL)

### Things to use
Ansible vault?
Modules rather than bare scripts eg for user provisioning
Component rather than apt for modules
ansible.cfg
Target localhost without hard coding

### Tools
ansible
git