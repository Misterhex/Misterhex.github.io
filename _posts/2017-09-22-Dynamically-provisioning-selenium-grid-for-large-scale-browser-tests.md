---
tags: devops selenium aws docker ansible terraform
---

Recently, as a `DevOps` consultant, I helped improve the performance of running large scale browser automation tests. 

The client's current infrastructure and setup was running the tests locally on their QA machines in a single windows server 2012 installed with selenium web driver. This setup is simplistic but unable to scale to large number of tests.

One good solution is using [selenium grid](https://github.com/SeleniumHQ/selenium/wiki/Grid2) to form a cluster of `webdrivers` to run test in parallel. Selenium grid allow multiple selenium drivers to work together. 

### SaaS alternatives
[Browserstack](https://www.browserstack.com) and [Saucelabs](https://saucelabs.com) are 2 commerical alternatives for running selenium tests. They provide great support a wide variety of platforms including various mobile devices. 

However, the pricing is steep, for example, *2* concurrent sessions is priced at $199 monthly. In my opinion, the commerical options is great for cross platform tests while running selenium grid on cloud providers is great for large scale functionality testing.

### Hub and Node
In selenium grid, there are 2 roles for the web drivers, `hub` and `node`. 

The `hub` is responsible for accepting all webdriver requests from the client. The `node` is a selenium-web-driver capable of running tests on various browsers. e.g. Chrome, Internet explorer, Firefox.

![cname file content]({{ site.url }}/assets/img/selenium_grid.png)

In the example grid above, it is setup with 1 hub and 7 nodes. Each node is capable of having 5 browsers sessions, and supports IE, Chrome and Firefox. With this setup, we can run *35* concurrent browser sessions.

### Connecting to the grid
From the selenium webdriver library, we simply instantiate the `RemoteWebDriver` with the `hub` address. 

E.g. `http://xxx.xxx.xxx.xxx:4444/wd/hub`

{% highlight java %}
DesiredCapabilities cap = DesiredCapabilities.chrome();
RemoteWebDriver driver =  new RemoteWebDriver(new URL("http://xxx.xxx.xxx.xxx:4444/wd/hub"), cap);
{% endhighlight %}

### Configuration
Both the hub and node can be easily configured through the cli or via json file. 

### Infrastructure
There are several factors in chosing what infrastructure to run the grid on. We explored various options.

#### Linux/ Docker or Windows?
There are considerations to make on which OS/platform to runs on, and or containers.

Firstly, if there is a need to run internet explorer browser, the nodes need to be provision on windows because IE only runs there.

If no such restriction exists, definitely use the [various selenium docker images](https://github.com/SeleniumHQ/docker-selenium) and run them in a container orchestrator like [kubernetes](https://kubernetes.io), [swarm](https://docs.docker.com/swarm/overview/) or [ecs](https://aws.amazon.com/ecs).

The docker route totally simplify the installation of the various drivers into a single image!

### Comparing various Web Drivers
Chrome and Firefox drivers are most common here because they strike a balance between performance and has modern webkits. 

[PhantomJs](http://phantomjs.org) is a good options if we want the tests to run fast because its a headless browser; it is faster because it doesn't really render the dom.

### Dynamically provisioning the grid
Since we need to have internet explorer support, we are running on windows. This means we can't go with the `docker` and `kubernetes` route.

Having a *50* windows servers sitting around idle in amazon is *expensive and non-cost optimized*. 

So as a major component of the project, we have build tools to automatically provisioned the nodes when needed and then de-provisioned the nodes when we are done running the tests. Our setup resolve around windows machines because our client need internet explorer. 

The tool does the followings:

1. use [terraform](https://www.terraform.io) to provision windows servers.

2. use [ansible-playbook](http://docs.ansible.com/ansible/latest/playbooks.html) and [ansible windows modules](http://docs.ansible.com/ansible/latest/intro_windows.html) to configure the hub and nodes with the various webdrivers and browsers, [win_chocolately](http://docs.ansible.com/ansible/latest/win_chocolatey_module.html) is really helpful here! 

3. Run the tests againsts the grid. 

4. terraform destroy when tests is completed to de-provisioned the nodes on our cloud provider.

### Conclusion
We then ran this tool as part of our client continous integration pipeline in jenkins. With this setup, we achieved a cost efficient solution for running large number of browser automation tests.