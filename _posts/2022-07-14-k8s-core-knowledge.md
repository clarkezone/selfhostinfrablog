---
title: Kubernetes Core knowledge
author: clarkezone
date: 2022-06-27 00:34:00 +0800
categories: [Kubernetes]
tags: [Metrics, Infrastructure]
---
# Draft - work in progress
## Situation
There are a set of basics that constitute a core set of knowledge for Kubernetes that I find myself using on a daily basis.

Container

### Kubernetes Cluster Components
Master Node
Worker Node

Manifest vs Resource

CNI, CSI, ?cluster interface

### Kubernetes Resource Types
Node
Pod
Namespace
Replicaset
Deployment
endpoint
Service
Ingress
Custom Resource Definition
ConfigMap
PersistentVolume
PersistentVolumeClaim
ServiceAccounts, Roles, Rolebindings
Certificate
Secret
Job

### Tutorials
Rancher youtube
Kubernetes the hardway TODO link
AKS the hardway TODO link

### Labels and metadata
look at knowledge

### Tools
wsl
git
docker (or podman)
Kubectl with autocompletion
k9s
Dashboard

### Managing and switching contexts

### Techniques / scenarios
Install Kube dashboard
Build container from dockerfile
Push container to container registry
Create and delete resources using kubectl
Create and delete resource from manifests
Edit resources
Get logs from pods / deployments
Describe deployments
Get yaml representation from a resource in cluster
Restart a deployment
Query and set default cluster storage provisioner
Exec into a pod