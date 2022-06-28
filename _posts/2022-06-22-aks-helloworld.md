---
title: From k3s to AKS
author: clarkezone
date: 2022-06-27 00:34:00 +0800
categories: [Kubernetes]
tags: [AKS]
---
## Situation
First AKS deployment

## Prerequisits to follow along
An Azure subscription
kubectl
kubectl autocompletion

For more detail on the above, see [[Kubernetes tools I used in 2022]] post

## Approach
> This article assumes you have a basic understanding some of simple kubernetes concepts 
> such as the basic resource types (pods, deployments, services, ingress) and have a 
> working familiarity with `kubectl`.

To get a basic cluster going in Azure you need to create a resource group, create a Log Analytics Workspace and then the cluster itself.

Here are some things I learned along the way