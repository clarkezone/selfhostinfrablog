---
layout: post
title:  "Using Okteto to run integration tests"
date: 2022-06-05 19:51:02 -0800
categories: [kubernetes, testing]
tags: [okteto, devproductivity]
---
If you are doing development targeting Kubernetes, you will know that the inner loop can be challenging.  The process is to typically work locally, create a image, push to a registry somewhere hopefully not too far away, update a kubernetes deployment manifest to point to the new version of the image, restart the deployment in the cluster, observe logs.  All of this is time and friction for iterating fast.

One of the additional challenges comes when the component you are developing is needs to not only run inside a cluster but also calls cluster API’s.  An extreme case is when you are building an extension like an operator.  

To help improve the inner loop in scenarios such as these various solutions have sprung up to solve this kind of problem.  One that I have looked into is Okteto.  I plan on doing a more extensive post in this in the future, for now I wanted to share how I’ve been leverage Okteto as a means of running and debugging some of my unit and integration tests that test code that creates job resources on the fly by integrating with the cluster API.

I've experimented with Okteto for a couple of use cases, firstly debugging code that integrates with the cluster API such as the job management logic.  Secondly, using Okteto as a mechanism for running integration tests that depend on kubernetes.

If you are inteterested in following along, grab the prevewd repo.  You can then try running one of the unit tests as follows:

1. thing
