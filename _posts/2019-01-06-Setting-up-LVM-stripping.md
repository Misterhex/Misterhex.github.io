---
title:  "Elasticsearch + LVM Striping"
tags: [devops, elasticsearch, lvm]
---

At work, while managing a fleet of elasticsearch clusters, we have several linux boxes that have inconsistent number of block devices attached. So to simplify the playbook, we don't want each hosts to have different `host_vars` and `path.data` settings, it will lead to configuration mess. 

So our solutions is to go with lvm, and create a single logically volume with striping enabled, out of multiple physical disks.

With this solution, for every data nodes in our cluster, we have elasticsearch writing to a common path. We used striping to increase performance. For data durability, elasticsearch sharding model already taken care of that so we don't need raid 10 properties.

some reference here on how to setup lvm:
[https://www.digitalocean.com/community/tutorials/an-introduction-to-lvm-concepts-terminology-and-operations](https://www.digitalocean.com/community/tutorials/an-introduction-to-lvm-concepts-terminology-and-operations) 


and some discussion around raid10 vs elasticsearch multiple data path performance.
[https://discuss.elastic.co/t/to-multi-path-data-or-hardware-raid-or-not/40143/3] (https://discuss.elastic.co/t/to-multi-path-data-or-hardware-raid-or-not/40143/3)