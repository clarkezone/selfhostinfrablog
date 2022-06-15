---
layout: post
title:  "Using Okteto to run integration tests"
date: 2022-06-05 19:51:02 -0800
categories: [kubernetes, testing]
tags: [okteto, devproductivity]
---
DRAFT5

If you are doing development targeting Kubernetes, you will know that the inner loop can be challenging.  The process is to typically work locally, create a image, push to a registry somewhere hopefully not too far away, update a kubernetes deployment manifest to point to the new version of the image, restart the deployment in the cluster, observe logs.  All of this is time and friction for iterating fast.

One of the additional challenges comes when the component you are developing is needs to not only run inside a cluster but also calls cluster API’s.  An extreme case is when you are building an extension like an operator.  

To help improve the inner loop in scenarios such as these various solutions have sprung up to solve this kind of problem.  One that I have looked into is Okteto.  I plan on doing a more extensive post in this in the future, for now I wanted to share how I’ve been leverage Okteto as a means of running and debugging some of my unit and integration tests that test code that creates job resources on the fly by integrating with the cluster API.

I've experimented with Okteto for a couple of use cases, firstly debugging code that integrates with the cluster API such as the job management logic.  Secondly, using Okteto as a mechanism for running integration tests that depend on kubernetes.

Challenges

1) Unit tests require environment variables to be set.  Those are currently not checked in, need to add .previewd_test.env
2) In machine settings, need to alter the path to point to the location in tree (can settings use %USERPROFILE?)
3) Because files are synced into Okteto, there is no git presense there hence need to hard code gitroot
4) Tests don't have an incluster mode hence necessary to hard code incluster mode
5) Can't start namespace watchers on either specific or default namespace.  This may be because i need a cluster role binding
7) Okteto cloud.. debugging takes around 10 seconds to start
8) Assuming a simple test worked, the end-2-end integration test requires ReadWriteMany volumes.  It's possible this could be worked around by restructuring test to create a seperate clone job against a readwriteone PVC followed by the webhook update trigger

The overall conclusion at this point is that Okteto isn't a good fit for this scenario

If you are inteterested in following along, grab the prevewd repo.  You can then try running one of the unit tests as follows:

1. enable unit tests by editing machine config (not user config) and adding:
```
{
    "go.buildFlags": [
         "-tags=unit,integration"
     ],
     "go.buildTags": "-tags=unit,integration",
     "go.testTags": "-tags=unit,integration"
}
```

