---
title: Case of the throttled daemonset
author: clarkezone
date: 2022-06-02 00:34:00 +0800
categories: [Kubernetes, Troubleshooting]
tags: [favicon]
---

Situation
I have two tiers of monitoring on my cluster.

Alert firing


Task
Observe the problem: 

Action
Theory: too conservative limits for node explorer.  First thing I tried was to increase first the ammount of CPU requested and second the limit:

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