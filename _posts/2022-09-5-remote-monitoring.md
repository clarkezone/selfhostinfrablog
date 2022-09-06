---
title: Remotely monitoring a Kubernetes cluster using Grafana cloud and Azure Container instances
author: clarkezone
date: 2022-09-21 00:34:00 +0800
categories: [Kubernetes, Grafana, Prometheus, Outage]
tags: [Observability]
---
# Draft - work in progress
## Situation
I recently had an outage on that staging instance of the blog that out host on my dev cluster.  As always from an outage, some good learnings, but without different layers of monitoring I wouldn't have known about it or prepared to response.  In this post I'll cover a little bit about the outage and what I learned and will lay out how the different layers of monitoring I have in place detected the incident and alerted me to what was going on.

## Approach
I have three distinct layers of observability in place on my blog website.  One at the application layer looking at individual microservices, one monitoring the kubernetes environment and infrastructure and one looking at a high level at the overall status and availability of the system from an external vantage point.  I have found that this combination gives me a good blend of coverage, some redundancy and also This post focusses on unpacking the second.

For the infrastrucutre monitoring layer I use two different solutions that do essentially the same thing.. one for the production cluster and one for the dev cluster.  I do this mainly to compare what's out there on the market and get a sense of pros and cons.  For the produciton environment I'm using Grafana Cloud and for Dev I'm using Azure Container Insights combined with Azure managed grafana.

## Grafana Cloud for Kubernetes

https://grafana.com/blog/2021/12/02/new-in-the-kubernetes-integration-for-grafana-cloud-curated-dashboards-built-in-alerts-and-more/

https://grafana.com/blog/2022/07/13/introducing-kubernetes-monitoring-in-grafana-cloud/

https://grafana.com/docs/grafana-cloud/kubernetes-monitoring/configuration/

Grafana Agent