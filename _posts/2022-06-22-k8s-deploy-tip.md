---
title: Tips for bootstrapping kubernetes workloads
author: clarkezone
date: 2022-06-21 00:34:00 +0800
categories: [Kubernetes]
tags: [manifests]
---
# Draft - work in progress
## Situation
Kubernetes manifests can be quite intimidating if you are coming from docker backrgound or are new to kubernetes.  Parituclarly if you are using the terminal, to quickly build a workload made up of a kubernetes deployment with service and ingress to kick the tires on a new application or container image it may seem like the only option is to copy from existing manifests or refer to the documentation.

Over time, I've learned that there is an easier way which is now my default method of quikly getting started setting up a new workload from the terminal.  In this post I'll show my approach which via a worked example of building out a test environment for the [Homer static homepage](https://github.com/bastienwirtz/homer) in my homelab.

## Prerequisits to follow along
k8s cluster (k3s)
kubectl
kubectl autocompletion
kubectl plugin system
klean plugin
k9s

For more detail on the above, see [[Kubernetes tools I used in 2022]] post

## Approach
> This article assumes you have a basic understanding some of simple kubernetes concepts 
> such as the basic resource types (pods, deployments, services, ingress) and have a 
> working familiarity with `kubectl`.

Kubernetes resources are most familiar in their textual form since `yaml` files are what you typically read about in articles, type in and manipulate day-to-day.  But thse manifests are really proxies for live objects representing reasources that live in the kubernetes resource database in your cluster.  One of the many nice features of `kubectl`  is the ability to convert from this live "object" form back to the textrual representation. So once you have an object you can get it back as yaml. 

## Steps
This article assumes you have a basic understanding some of simple kubernetes concepts such as resource types [pods](https://kubernetes.io/docs/concepts/workloads/pods/), [deployments](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/), [services](https://kubernetes.io/docs/concepts/services-networking/service/) and have a working familiarity with kubectl

1. Create a deployment
`kubectl create deployment homer-deployment --image b4bz/homer`

2. Create clean manifests for the deployment
`kubectl get deployment homer-deployment -o yaml > kubectl neat > homer-deployment.yaml`

3. Delete resource
4. Remove extranious values from manifest 

3. Edit the saved manifest, name of deployment, and app mentadata that the service will use. Make note of the app name

3. Create a service
`kubectl create service clusterip homer-service --tcp 8080`

4. create clean manifest for service  `kubectl get deployment homer-service -o yaml > `kubectl neat > homer-service.yaml`

delete resource

5. edit the app that the service points to
6. get rid of hard coded cluster ip

3. Test frontend via port forwarding
`kubectl port-forward services/homer-service 8085:8080`

5. Create an ingress

6. config files