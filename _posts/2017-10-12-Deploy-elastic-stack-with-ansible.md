In my devops role, we adopt infrastructure as code, all our configuration of software are automated using `ansible`. I am using `ansible` for a lot of configuration management scripts. 

One of the configuration scripts I did was configuring the `elastic stack`. 

This scripts is availabe at my github repo, [https://github.com/Misterhex/ansible-elk](https://github.com/Misterhex/ansible-elk)

This scripts configure:
- elasticsearch cluster with N number of nodes.
- kibana
- curator
- metricbeats on all compute nodes.

Below is the main playbook file `site.yml`

{% highlight yaml %}
---
- hosts: all
  gather_facts: false
  become: true
  roles:
    - python

- hosts: elasticsearch
  become: true
  vars:
    cluster_name: openstack_test
    es_heap_size: 2g
  roles:
    - java8
    - elastic5x
    - elasticsearch

- hosts: kibana
  become: true
  vars:
    kibana_port: 5601
  roles:
    - elastic5x
    - kibana

- hosts: compute
  become: true
  roles:
    - elastic5x
    - metricbeat
    - filebeat

- hosts: curator
  become: true
  vars:
    delete_older_than_days: 1
  roles:
    - curator

- hosts: compute[0]
  tasks: 
  - name: import metricbeats dashboards to kibana
    shell: /usr/share/metricbeat/scripts/import_dashboards
  - name: import filebeat dashboards to kibana
    shell: /usr/share/filebeat/scripts/import_dashboards
{% endhighlight %}