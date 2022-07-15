---
layout: post
title:  "State of the Tailscale tailnet 2022 edition"
date: 2022-07-09 19:51:02 -0800
categories: [tailscale]
tags: [tailnet]
image: Broadcast_Mail.png
---

A couple of exchanges about tailscale this week have prompted me to document the state of my tailnet.  What is a tailnet you may ask.  Well, dear reader, a tailnet is esentially a private VPN connecting your devices into a mesh.  With a tailscale install on each device it is possible to seemlessly connect and transfer files between them even if they are located on diverse, private networks, datacenters etc.  I've been using Tailscale for a while now and have come to depend on the services on a daily basis.

Devices

Existing Use cases

1. Remote access to devices on home network
Dev VM - SSH, coder-server
Synology
Main desktop RDP
2. Remote access to workloads self-hosted on homelab k8s clusters
3. Access cloud hosted resources
4. File transfer between devices

Future use cases 

1. Calling from Github CI/CD actions to self-hosted resources
2. Connecting to cloud hosted kubernetes clusters