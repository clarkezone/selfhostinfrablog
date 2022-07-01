---
title: A minimal install of Grafana
author: clarkezone
date: 2022-06-27 00:34:00 +0800
categories: [Grafana]
tags: [Dashboard]
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

## Minimal Grafana
Many examples of grafana installed on kubernetes, including prom-kube install that you want the full range of kubernetes metrics and dashboards.  For my usecase, I want a minimal install of grafana to display my logs stored in loki as well as my application metrics.  So, I really just want grafana and a few custom dashboards.

Article that helped: [https://rpi4cluster.com/monitoring/monitor-intro/](https://rpi4cluster.com/monitoring/monitor-intro/)

1. 