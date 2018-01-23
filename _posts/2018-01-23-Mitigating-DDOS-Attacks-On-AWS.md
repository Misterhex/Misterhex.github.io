---
title:  "Mitigating DDOS attacks on AWS"
tags: [devops, aws, architecture]
---

Recently, a friend asked about how to handle ddos attacks on AWS. We discussed and this are some pointers. Lets say there are 3 components; nodejs api, redis, mysql.

Basic architectures setup:
1. register the domain with route53 name servers for highly available dns.
2. point route53 to elastic load balancer.
3. elb points to nodejs servers.
4. nodejs api servers should sit in private subnet routed to NAT gateway. ( for outbound http calls to other services or downloading of apt / yum packages.
5. each components should have its own security groups; nodejs, redis, mysql.

Mitigate ddos attacks:
1. ensure nodejs web apps is design to be horizontally scalable. ( follow 12factor app design https://12factor.net )
2. use auto scaling group to take traffic spike.
3. use durable message queues in the architecture to smoothen the spike requests to other backend services.
4. use aws sheid for ddos protection; to detect ddos traffic patterns.
5. authenticate caller, and rate limit the api per caller.
6. enable vpc flow log to detect anomaly
7. use monitoring tools to detect attacks and trigger off alerts. ( elastalert, prometheus/grafana )
8. only exposing route53 and elastic load balancer to be public facing allow aws to take care of infrastructure availability.

Accessing the servers:
1. using either vpn and bastion, they would sit in the public subnet allowing ssh. then ssh agent forward to access web, mysql, redis servers.