---
title: "Knative Introduction"
date: 2021-07-10T14:44:11-07:00
draft: false
---

## Introduction

The knative project is very interesting as I work on an always on ML Service called log anomaly detector. One idea came to mind is to perform the machine learning encoding on logs that come in and have it scale to zero when no data is streamed in. Also to have the system scale up as the demand increases. If I was running something like this on the cloud where I’m charged by the minute then I’d use serverless as a way to cut costs.

In this blog post I’ll share my notes on serverless in particular Knative and what I’ve learned.

## What is Serverless

It can mean different things for different people but for me these are the characteristics that got me interested in exploring serverless:

Instead of building large monolithic applications you can build functions that are invoked by events.
It could be as simple as an http request or a message from a message broker like Google Pub/Sub
Events like upload data to s3 or updating database row.
Only paying for active compute time rather then always on Daemon.

## KNative (brief description):

* It lets you focus on development rather than the platform.
* Knative is a serverless opensource option for LAMBDA by AWS.
* Has three main components:
    * Serving
    * Eventing
    * Build

## Knative Components
#### Knative Serving
* Scale-to-zero for app services which leverages FaaS
* Offers point in time snapshot.
* Go from container -> URL
* Handles the scaling from to and from zero.

#### Knative Eventing
* A mechanism for eventing from sources to sink through channels. Events can trigger services using a sinks to scale up from zero
* Serverless is a event driven architecture
* Rather than your app focus on events then you can let Knative deliver the event.
* Sources: (See knative events documentation)
    * Source of the event
    * Kube-events
    * Github events
    * Container source
* Channel
* Subscription

#### Knative Build
* For compiling source code
* From Source code -> Container
* Think of this like a drop in replacement for S2i
* Might be replaced by tekton pipelines
* You can use Kaniko. TO build container image without requiring docker daemon. Which requires root access. Has support for Google’s Kaniko so you can build containers on kubernetes without elevated access.

## Why use Knative ?

Let’s you build your application, serve traffic to it, and enable applications to easily consume and produce events. In a cloud environment if your goal is to reduce cost then you may want to scale down services when they are not being used or not serving traffic. As resources are expensive and often clouds charge by the minute for the hardware CPU/GPU/Ram etc.

#### Prerequisites
Kubernetes 1.11 or greater
Currently there are three ingress/gateway:
Install Istio or Ambasador or gloo:
Traffic management

#### Terminology:
| concept                  | Definition |
|--------------------------|------------|
| Knative Serving Revision | Its a snapshot of users code and or config running in the container.       |
| native Serving Route  | Exposes revisions to clients via ingress/gateway rule        |
| Kubernetes Deployment |   You deploy a single pod that will run your container which points to a revision     |
| Knative Serving Autoscaler  |   Watches requests load on the pod and increases or decreases the size of the deployment in order to handle more or less traffic.  |
| Knative Serving Activator |      Watches for requests for revisions of with no running pods. It brings up the pod via revision controller and forwards caught requests.      |
 
	
	
	
#### States
* Active: When they are actively serving requests
* Reserved when they are scaled down to 0 pods but still in service
* Retired when the pod no longer will receive traffic.


### ElasticScale:
* Perform scaling based on load automatically
* Problem:
    * Kubernetes can analyze external load and capacity related events to scale out as desired.
    * There are two main approaches to scalability:
        * Vertical:
            * Give more resources to pod.
                * Faster CPU, GPU, RAM, SSD
        * Horizontal:
            * Give more replicas to pod or more nodes
    * Creating an autoscaler is complex.

### Manual Scaling:
* kubectl scale random –replicas=4
* A job can be scaled to run multiple nodes.


Example of creating a serving component here:

```yaml
apiVersion: serving.knative.dev/v1alpha1
kind: Service
metadata:
  name: testing-demo
  namespace: default
  labels:
   serving.knative.dev/visibility: cluster-local
spec:
  template:
    spec:
      containers:
        – image: quay.io/zmhassan/helloworld:v1
          env:
            – name: TARGET
              value: "v1"

```




