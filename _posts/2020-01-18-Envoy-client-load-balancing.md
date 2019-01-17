---
title:  "Using Envoy Proxy for client side load balancing"
tags: [devops, envoy, elasticsearch]
---

We are using [Envoy](https://www.envoyproxy.io) for client side load balancing.

We use [elasticsearch exporter](https://github.com/justwatchcom/elasticsearch_exporter) for getting prometheus format metrics for monitoring our clusters. 
Our elasticsearch cluster is not configured behind a load balancer, and we don't have dns pointing to the multiple 
So we needed a way to allow smart client side load balancing, and most application doesn't have this.

So rather than making this an application layer concerns, we can move the concerns to the network layers.

By using envoy, which is a out-of-process proxy, we can easily implement client side load balancing, and also move tls concerns out ( TLS origination ),

Envoy can do health checks, and passive ejections of endpoints.

With just a simple envoy side car running beside elasticsearch exporter, we can pull metrics in a highly available manner from a list of elasticsearch nodes, because the metrics are all the same from whichever nodes we query.

Envoy is amazing, and seeing adoption in many large projects, e.g. [istio](http://istio.io), [aws app mesh](https://aws.amazon.com/app-mesh/). 

It is also an enabler for a lot of capabilities, such as fault injections, traffic mirroring and shifting for safe canaries deployment.

Highly recommended to check it out! [http://envoyproxy.io](http://envoyproxy.io)