---
title: Case of the throttled daemonset
author: clarkezone
date: 2022-06-02 00:34:00 +0800
categories: [Kubernetes, Troubleshooting]
tags: [favicon]
---

## Situation
I have two tiers of monitoring on my cluster.

Around about late May 2022, an alert started firing.


## Task
Theory: too conservative limits for node explorer.  First thing I tried was to increase first the ammount of CPU requested and second the limit:

The approach I came up with to confirm theory and fix alert:

1. Observe the problem in the cluster and understand exactly what is throttled and by how much.  In the process get my head round CPU reests / limis in particular what the various numbers and percentages actually mean
2. Confirm understanding by changing limits to confirm I've pinpointed the correct entity and values.
3. Find root cause.. what is putting load on the node exporter to cause it to exceed limit

## Action

```
diff --git a/manifests/monitoring/manifests/nodeExporter-daemonset.yaml b/manifests/monitoring/manifests/nodeExporter-daemonset.yaml
index e3901b0..a33cd39 100644
--- a/manifests/monitoring/manifests/nodeExporter-daemonset.yaml
+++ b/manifests/monitoring/manifests/nodeExporter-daemonset.yaml
@@ -38,10 +38,11 @@ spec:
         name: node-exporter
         resources:
           limits:
-            cpu: 250m
+            cpu: 500m
             memory: 180Mi
           requests:
-            cpu: 102m
+            #            cpu: 102m
+            cpu: 250m
             memory: 180Mi
         volumeMounts:
         - mountPath: /host/sys
```

Outcome