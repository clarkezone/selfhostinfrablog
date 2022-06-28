---
title: A minimal install of Prometheus using the Prometheus Operator
author: clarkezone
date: 2022-06-27 00:34:00 +0800
categories: [Prometheus]
tags: [Metrics, Infrastructure]
---
## Situation
Getting your self-hosting infrastructure configured with the appropriate observability stack is a key consideration.  I've been investigating different approachs to this including the kube-prometheus stack and the prometheus operator.  This post talks about the latter.

## Prerequisits to follow along
A test cluster
kubectl
kubectl autocompletion

For more detail on the above, see [[Kubernetes tools I used in 2022]] post

## Approach
> This article assumes you have a basic understanding some of simple kubernetes concepts 
> such as the basic resource types (pods, deployments, services, ingress) and have a 
> working familiarity with `kubectl`.

## Kube-prometheus vs the Prometheus operator
One of the things that confused me about these two approachs is that they are often disussed together and the documentation doesn't have examples that contrast between then.

Documentation: [https://prometheus-operator.dev](https://prometheus-operator.dev)
Github repo: [https://github.com/prometheus-operator/prometheus-operator]9https://github.com/prometheus-operator/prometheus-operator)


## Steps
1. If you have previously used the kube-prometheus instructions delete from manifests
```bash
kubectl delete -f .
``` 
> If you have a previous install and attempt to delete by deleting namespace you may experiance a hung namespace
> which manifests (no pun intended) as stuck in deleting
> Get cluster API status.. whoops this thing is missing hence api server failing
> delete from files
2. Delete previous CRDs

## Install Kubernetes operator from Bundle
1. Clone the bundle
```bash
# todo 
```
2. 

```bash
kubectl create -f bundle.yaml
```

