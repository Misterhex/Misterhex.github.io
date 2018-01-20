---
title:  "Running Twemproxy In Kubernetes"
tags: [devops, kubernetes]
---

In this short post, i would like to how to setup [twemproxy]() for [redis]() cluster sharding running in [kubernetes]().

## Setup
These are the few tasks required:

- Running `twemproxy` as a single replica deployment in `kubernetes`.
- Running `redis` replicas as [statefulsets]() with  with in `kubernetes`.
- Allowing other `pods` in `kubernetes` to access `twemproxy`.
- Allowing `twemproxy` to connect to the redis's `pods`.

## Twemproxy
We can run twemproxy as a single `deployment` in `kubernetes`. We can use the following community image [jgoodall/twemproxy](https://registry.hub.docker.com/u/jgoodall/twemproxy/)


{% highlight yaml %}
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  labels:
    app: twemproxy
  name: twemproxy
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: twemproxy
    spec:
      containers:
      - image: jgoodall/twemproxy:latest
        name: twemproxy
        ports:
        - containerPort: 63791
        - containerPort: 63792
        - containerPort: 6222
      restartPolicy: Always
{% endhighlight %}