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
Capabilites
Accounts and lockdown
Tailscale
Devbox setup
k3s

Devbox on Linux hosted in the cloud or Hyper-V
Devbox on WSL
Remote monitoring server
Master and worker nodes on home k3s clusters

### Kubernetes Resource Types

### Tools
ansible
git