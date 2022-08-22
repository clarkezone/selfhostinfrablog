---
title: Monitoring cluster state through remote synthetic probes with CloudProber
author: clarkezone
date: 2022-08-21 00:34:00 +0800
categories: [Kubernetes, CloudProber, Prometheus]
tags: [manifests]
---
# Draft - work in progress
## Situation
Uptime is important for services.  And when running services for yourself, whilst you aren't tied to any SLA's or SLO's it's important to reliably know the state of your systems. For this kind of scenario synthetic probes are often used to provide fake traffic that simulates realworld use.  In order for the fake traffic to have enough realism to actually test the scenario, the obviously the requests need to be originating from outside the network hosting the services.

For homelab scenario 

## Prerequisits to follow along
k8s cluster (k3s)
kubectl
kubectl autocompletion
kubectl plugin system
klean plugin
k9s

For more detail on the above, see [[Kubernetes tools I used in 2022]] post

## Approach
