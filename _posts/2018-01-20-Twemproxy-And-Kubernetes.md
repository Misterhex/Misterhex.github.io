---
title:  "Running Twemproxy In Kubernetes"
tags: [devops, kubernetes]
---

In this post, i would like share on how to setup [twemproxy]() for [redis]() cluster sharding running in [kubernetes]().

## Setup
These are the tasks required:

- Running `twemproxy` as a single replica deployment in `kubernetes`.
- Running `redis` replicas as [statefulsets]() with  with in `kubernetes`.
- Allowing other `pods` in `kubernetes` to access `twemproxy`.
- Allowing `twemproxy` to connect to the redis's `pods`.

## Twemproxy
We can run twemproxy as a single `deployment` in `kubernetes`. We can use the following community image [jgoodall/twemproxy](https://registry.hub.docker.com/u/jgoodall/twemproxy/)

twemproxy's deployment & service spec:
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
---
apiVersion: v1
kind: Service
metadata:
  labels:
    app: twemproxy
  name: twemproxy
spec:
  selector:
    app: twemproxy
  ports:
  - name: "63791"
    port: 63791
    targetPort: 63791
  - name: "63792"
    port: 63792
    targetPort: 63792
  - name: "6222"
    port: 6222
    targetPort: 6222
{% endhighlight %}

The configuration above will deploy twemproxy's `deployment` and `service`. `service` in kubernetes allow pods to network with each other, this service will allow other pods to reach `twemproxy`.

twemproxy.yml:
{% highlight yaml %}
local:
  listen: 0.0.0.0:63791
  hash: fnv1a_64
  hash_tag: "{}"
  distribution: ketama
  timeout: 500
  redis: true
  servers:
    - redis-0:6379:1
    - redis-1:6379:1
    - redis-2:6379:1

session:
  listen: 0.0.0.0:63792
  hash: fnv1a_64
  hash_tag: "{}"
  distribution: ketama
  auto_eject_hosts: true
  server_retry_timeout: 30000
  server_failure_limit: 3
  timeout: 500
  redis: true
  servers:
    - redis-0:6379:1
    - redis-1:6379:1
    - redis-2:6379:1
{% endhighlight %}

Above is twemproxy's configuration which need to be mount as `configmap volume` in twemproxy's deployment.

## Redis Statefulset
Next, we need to create redis's statefulset. Statefulset with 3 replicas will create the following pods `redis-0`, `redis-1` and `redis-2`.  Pods created by statefulset will be labeled as `statefulset.kubernetes.io/pod-name: {{pod name}}`, we can use this as selector for the `service`. Unfortunably, we will still need to create 3 services to allow each pod to be addressable by `twemproxy`, 

redis statefulset:
{% highlight yaml %}
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: redis
spec:
  selector:
    matchLabels:
      app: redis
  serviceName: "redis"
  replicas: 3
  template:
    metadata:
      labels:
        app: redis
    spec:
      terminationGracePeriodSeconds: 3
      containers:
      - name: redis
        image: redis:3.0.7
        ports:
        - containerPort: 6379
{% endhighlight %}

redis services:
{% highlight yaml %}
apiVersion: v1
kind: Service
metadata:
  name: redis
  labels:
    app: redis
spec:
  ports:
  - port: 6379
    name: redis
  clusterIP: None
  selector:
    app: redis
---
apiVersion: v1
kind: Service
metadata:
  name: redis-1
  labels:
    app: redis-1
spec:
  ports:
  - port: 6379
    name: redis-1
  clusterIP: None
  selector:
    statefulset.kubernetes.io/pod-name: redis-1
---
apiVersion: v1
kind: Service
metadata:
  name: redis-1
  labels:
    app: redis-1
spec:
  ports:
  - port: 6379
    name: redis-1
  clusterIP: None
  selector:
    statefulset.kubernetes.io/pod-name: redis-1
---
apiVersion: v1
kind: Service
metadata:
  name: redis-2
  labels:
    app: redis-2
spec:
  ports:
  - port: 6379
    name: redis-2
  clusterIP: None
  selector:
    statefulset.kubernetes.io/pod-name: redis-2
{% endhighlight %}