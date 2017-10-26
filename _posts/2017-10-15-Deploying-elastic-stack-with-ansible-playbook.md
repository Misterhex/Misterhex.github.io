---
tags: devops elasticsearch ansible
---

In my devops role, we adopt the principles of [infrastructure as code](https://www.thoughtworks.com/insights/blog/infrastructure-code-reason-smile). With the motivation to automate as much infrastructure work as possible, many of our configuration are automated using [ansible](https://www.ansible.com) and `ansible playbook`. I am using `ansible` for a lot of configuration management scripts. 

One of the configuration scripts I did was for automating the configuration of the [elastic stack](https://www.elastic.co).

This script is availabe at my github repo, [https://github.com/Misterhex/ansible-elk](https://github.com/Misterhex/ansible-elk)

This script configure:
- elasticsearch cluster with N number of nodes.
- kibana
- curator
- metricbeats on all compute nodes.
- import default dashboard for metricbeat

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

### Concepts
Playbook contains `plays` that will run when executed, `plays` maps to `inventory`. `Plays` contains `Roles`. `Roles` are basically recommended encapsulation of a series of `steps` that invoke ansible `modules`. `Modules` are built-in [idempontent](https://stackoverflow.com/a/1077421/1610747) functions that configure the target servers. Servers are grouped and defined in `inventories`, they are defined in an `inventory file`.

Below is an example inventory file:
{% highlight ini %}
[all_servers:children]
compute
elasticsearch
kibana

[all_servers:vars]
ansible_user = ubuntu
ansible_ssh_private_key_file = xxx

[compute]
xxx.xxx.xxx.xxx

[elasticsearch]
xxx.xxx.xxx.xxx

[kibana]
xxx.xxx.xxx.xxx
{% endhighlight %}

### Running the playbook
To run the playbook, it is as simple as:
```
ansible-playbook -i inventory-sample site.yml
```

### Conclusion 
In conclusion, ansible is a simple yet awesome tools for automating configuration of softwares/services. Get started with installation [here](http://docs.ansible.com/ansible/latest/intro_installation.html).