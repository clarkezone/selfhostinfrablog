---
title: Bad istio uninstall broke my Prometheus
author: clarkezone
date: 2022-07-6 00:34:00 +0800
categories: [Prometheus]
tags: [Metrics, Infrastructure]
---
# Draft - work in progress
## Situation
I work up one morning and something had killed my prometheus, specifically a bad Istio uninstall.  I knew this because I'd made no changes but visiting to my usual grafana dashboards

Doing a bit of digging, prometheus operator was failing to start

```bash
kujbectl get deployments -n monitoring

NAME                  READY   UP-TO-DATE   AVAILABLE   AGE
prometheus-operator   0/1     0            0           20m

kubectl describe replicasets.apps -n monitoring

Events:
  Type     Reason        Age                   From                   Message
  ----     ------        ----                  ----                   -------
  Warning  FailedCreate  68s (x15 over 2m30s)  replicaset-controller  Error creating: Internal error occurred: failed calling webhook "auto.sidecar-injector.istio.io": Post "https://istiod.istio-system.svc:443/inject?timeout=10s": service "istiod" not found
``` 

So what to do?  Turns out although I had uninstalled the experimental faas solution, bits of istio were clearly left behind.  

Quick look at the api resources bore this out:

```bash
monitoring git:(monitoring) ✗ kubectl get customresourcedefinitions.apiextensions.k8s.io | grep istio                           
authorizationpolicies.security.istio.io     2022-07-05T16:02:36Z 
destinationrules.networking.istio.io        2022-07-05T16:02:37Z 
envoyfilters.networking.istio.io            2022-07-05T16:02:37Z                                                                   
gateways.networking.istio.io                2022-07-05T16:02:37Z                                                                   
peerauthentications.security.istio.io       2022-07-05T16:02:38Z 
proxyconfigs.networking.istio.io            2022-07-05T16:02:38Z                                                                   
requestauthentications.security.istio.io    2022-07-05T16:02:39Z                                                                   
serviceentries.networking.istio.io          2022-07-05T16:02:39Z                                                                   
sidecars.networking.istio.io                2022-07-05T16:02:39Z                                                                   
telemetries.telemetry.istio.io              2022-07-05T16:02:40Z                                                                   
virtualservices.networking.istio.io         2022-07-05T16:02:40Z                                                                   
wasmplugins.extensions.istio.io             2022-07-05T16:02:40Z                                                                   
workloadentries.networking.istio.io         2022-07-05T16:02:40Z                                                                   
workloadgroups.networking.istio.io          2022-07-05T16:02:41Z
```

At this point I found myself thanking my lucky stars that the experimentation I'd been doing with faas solutions was on a firewalled dev cluster that couldn't disrupt production.

Still no dice.  So time to dig in on 

From this post: https://stackoverflow.com/questions/51489955/how-to-obtain-the-enable-admission-controller-list-in-kubernetes

```bash
kubectl get --raw /apis/admissionregistration.k8s.io/v1/validatingwebhookconfigurations | jq
```

```bash
kubectl get --raw /apis/admissionregistration.k8s.io/v1/validatingwebhookconfigurations | jq '.items[] | select(.metadata.name=="istio-validator-istio-system")'
```

Ha ha, there is the culprit

From there we can nuke that sucker with 

```bash
kubectl delete validatingwebhookconfigurations.admissionregistration.k8s.io istio-validator-istio-system 
```

Err no still no dice.  Finally with more digging more carefully reading this documentation:

https://istio.io/latest/docs/ops/configuration/mesh/injection-concepts/

Seems that the culprit is mutating webhook

```bash
kubectl get mutatingwebhookconfigurations.admissionregistration.k8s.io

NAME                     WEBHOOKS   AGE
cert-manager-webhook     1          214d
istio-sidecar-injector   5          4d

```

Finally.. we are unblocked.


