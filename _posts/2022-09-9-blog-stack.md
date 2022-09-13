---
title: Production Blog hosting on K3s using Previewd
author: clarkezone
date: 2022-09-21 00:34:00 +0800
categories: [Kubernetes, Previewd, Prometheus, ]
tags: [Blogging]
---
# Draft - work in progress
## Situation
The project powering this blog is previewd, this post talks about the production kubernetes environment I use to host that.

## Initial Approach
- Frontend
- Webhook / job dispatch
- Telemetry
- Ingress

PersistentVolumes
- src
- render

Webhook server Deployment
- container: previewd

Front end Deployment
- container: nginx
- container: cloudflared

## Outage learnings
The initial approach worked but had a flaw that caused an outage during testing which caused me to make an important change.

Longhorn vs Local storage

Since my intent was to have multiple nginx instances to facilitate scaling and redundancy, it seemed logical to store the rendered website content on a Longhorn volume to enable sharing between nginx instances no matter which physical node they were located on.  This turned out to be a mistake.  During the “outage”, I lost a physical cluster node (it hung and needed to be rebooted).  During this event, the longhorn cluster lost quorum causing the volume to go offline.  This took out all of the nodes.  The learning here was that every instance of nginx needs to have a copy of the rendered web content such that if other dependencies fail, that instance can continue to serve traffic.  To accomplish this goal, I opted for a solution that would synchronize content from the central longhorn volume to a local volume, and then have nginx serve from the local volume.

Lsyncd and Local Path Provisioner to the rescue 

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: local-path-pvc
  namespace: default
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: local-path
  resources:
    requests:
      storage: 1Gi
``` 

or.. is it EmptyDir to the rescue instead?

Because a single node may have multiple nginx instances, I needed to come up with a local directory structure that enabled sharing web content to avoid every replica having it’s own

## Manifests

Frontend
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: frontenddeployment
spec:
  strategy:
    type: Recreate
  selector:
    matchLabels:
      app: jekyllpreview
  template:
    metadata:
    spec:
      serviceAccountName: previewd-sa 
      containers:
      - name: localsync
        image: todo
      - name: blog-serve
        image: nginx:1.20-alpine
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: “/usr/share/nginx/html”
          name: blogrender
          readOnly: true
        - mountPath: /etc/nginx/conf.d/default.conf
          readOnly: true
          name: nginxconfig
          subPath: default.conf
      - name: nginx-exporter
        image: nginx/nginx-prometheus-exporter:0.10.0
        resources:
          requests:
            cpu: 100m
            memory: 100Mi
        ports:
        - name: nginx-ex-port
          containerPort: 9113
        args:
          - -nginx.scrape-uri=http://localhost:80/nginx_status
        volumeMounts:
          - mountPath: /src
            name: blogsource
        ports:
        - containerPort: 8090
      volumes:
        - name: blogrender
          persistentVolumeClaim:
            claimName: blogrender-pvc
        - name: nginxconfig
          configMap:
            name: nginx-cm
```  


