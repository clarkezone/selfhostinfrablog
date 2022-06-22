---
title: Tips for bootstrapping kubernetes manifests
author: clarkezone
date: 2022-06-21 00:34:00 +0800
categories: [Kubernetes]
tags: [manifests]
---
## Situation
Kubernetes manifests can be quite intimidating if you are coming from docker backrgound and/or are new to kubernetes.  If you want to quickly build a kubernetes deployment / service to kick the tires on a new application / container image, I've learned that there is an easier way than having to hand author said manifests.  Here I'll show my approach which I recently applied to the scenario of building out a test environment for the Homer project TODO

## Prerequisits to follow this tutorial
k8s cluster (k3s)
kubectl
kubectl autocompletion
kubectl plugin system
klean plugin
k9s

For more detail on the above, see [[Kubernetes tools I used in 2022]] post

## Approach
This article assumes you have a basic understanding some of simple kubernetes concepts such as resource types pods, deployments, services and have a working familiarity with kubectl

1. Create a deployment
`kubectl create deployment homer-deployment --image b4bz/homer`

2. Create a service
`kubectl create service clusterip homer-service --tcp 8080

3. Test frontend via port forwarding
`kubectl port-forward services/homer-service 8085:8080`

4. Create clean manifests
`kubectl get deployment homer-deployment -o yaml > kubectl neat > homer-deployment.yaml`
`kubectl get deployment homer-service -o yaml > kubectl neat > homer-service.yaml`

5. Create an ingress
