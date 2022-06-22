---
title: Tips for bootstrapping kubernetes manifests
author: clarkezone
date: 2022-06-21 00:34:00 +0800
categories: [Kubernetes]
tags: [manifests]
---
## Situation
In my journey into kubernetes over the last couple of years, I've picked up a number of tips to accerate the creation of basic manifests to boostrap an application in my home clusters.  I recently wanted to kick the tyres on Homer, a simple homepage system for indexing services on a home cluster and thought I'd write the steps down in case they are of use to others.

## Prerequisits
k8s cluster (k3s)
kubectl
kubectl autocompletion
kubectl plugin system
klean plugin
k9s

For more detail on the above, see my [[Kubernetes tools I used in 2022]] post

This article assumes you understand some of the basic kubernetes resource types such as  pods, deployments, services and have a working familiarity with kubectl

** Approach
Kubernetes manifests can be quite intimidating if you are coming from docker and/or are new to kubernetes.

1. Create a deployment
`kubectl create deployment homer-deployment --image b4bz/homer`

2. Create a service
`kubectl create service clusterip homer-service --tcp 8080

3. Test frontend via port forwarding

4. Create clean manifests`

5. Create an ingress
