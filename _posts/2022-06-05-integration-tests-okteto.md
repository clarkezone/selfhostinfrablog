---
layout: post
title:  "Using Okteto to run integration tests"
date: 2022-06-05 19:51:02 -0800
categories: [kubernetes, testing]
tags: [okteto, devproductivity]
---
If you are doing development targeting Kubernetes, you will know that the inner loop can be challenging.  The process is typically work locally, create a image, push to a registry somewhere hopefully not too far away, update a kubernetes deployment manifest to point to new tag, restart the deployment in the cluster, observe logs.



```cs
class test {
    string hello;
}
```

**Also looking at editing workflows**

Working Copy and 1writer seems like a pretty great combo.  Amazing.
