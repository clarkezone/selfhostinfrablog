---
title: A minimal install of Prometheus using the Prometheus Operator
author: clarkezone
date: 2022-06-27 00:34:00 +0800
categories: [Prometheus]
tags: [Metrics, Infrastructure]
---
# Draft - work in progress
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
Github repo: [https://github.com/prometheus-operator/prometheus-operator](https://github.com/prometheus-operator/prometheus-operator)
Article that helped: [https://rpi4cluster.com/monitoring/monitor-intro/](https://rpi4cluster.com/monitoring/monitor-intro/)


## Uninstall
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
1. grab the bundle
```bash
curl -LO https://raw.githubusercontent.com/prometheus-operator/prometheus-operator/v0.52.0/bundle.yaml
```
2. Install

```bash
kubectl create -f bundle.yaml
```

> This will install the prometheus operator into the default namespace.  If you want to use > a different namespace, you'll need to do update the namespace by doing something similar > to `sed -i 's/namespace: default/namespace: monitoring/g' bundle.yaml` 

The install will result 

4. Deploy a simple prometheus instance

This requires the following kubernetes objects:
- ServiceAccount
- ClusterRole
- ClusterRoleBinding
- Prometheus
- Ingress (ommitted from this example)

The Prometheus custom resource is the key.  It leverages the prometheus operator to create a prometheus instance 

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  labels:
    app: prometheus-instance
  name: prometheus-instance
  namespace: monitoring
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  labels:
    app: prometheus-instance
  name: prometheus-instance
rules:
- apiGroups:
  - ""
  resources:
  - nodes
  - nodes/metrics
  - services
  - endpoints
  - pods
  verbs:
  - get
  - list
  - watch
- apiGroups:
  - ""
  resources:
  - configmaps
  verbs:
  - get
- apiGroups:
  - networking.k8s.io
  resources:
  - ingresses
  verbs:
  - get
  - list
  - watch
- nonResourceURLs:
  - /metrics
  verbs:
  - get
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  labels:
    app: prometheus-instance
  name: prometheus-instance
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: prometheus-instance
subjects:
- kind: ServiceAccount
  name: prometheus-instance
  namespace: monitoring
---
apiVersion: monitoring.coreos.com/v1
kind: Prometheus
metadata:
  labels:
    app: prometheus-instance
  name: prometheus-deployement
  namespace: monitoring
spec:
  enableAdminAPI: true
  resources:
    requests:
      memory: 400Mi
  serviceAccountName: prometheus-instance
  serviceMonitorNamespaceSelector: {}
  serviceMonitorSelector: {}
```

If you apply the above, you should see the following resources created in the monitoring namespace.


Key lines are serviceMonitorNamespaceSelector which will pick up service monitor configurations.

Let's look at an example of one of those
 
Test it using previewd webserver
Service monitor for previewd


## Alerting
In order to get alerting working, we need to tweek the prometheus instance manifest

```
ruleSelector: {}
```

This will now pick up rules such as the following:

```
todo rule
```

Install grafana with just my options, persistent storage, default point to prom and loki, default un and password, 

