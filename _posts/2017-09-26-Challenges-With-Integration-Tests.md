I recently read this really informative [article](https://watirmelon.blog/2012/01/31/introducing-the-software-testing-ice-cream-cone/) on integration tests anti pattern. What triggers me to look into more of this is a situation with my consulting client, they have a culture that falls exactly into this anti pattern known as the testing ice cream cone.

![cname file content]({{ site.url }}/assets/img/softwaretestingicecreamconeantipattern.png)

As illustrated in the diagram above, from top to bottom, the organization focus majority of its resources & effort on manual testings and least on unit tests. This is a very common pattern i have seen throughout our career. 

### Why is this pattern bad?
Heavy focusing and reliance on manual testing is a slow, non-scalable & non-repeatable and tedious process. Manual testers have to repeatly check features that are developed in this sprint and also in the last N sprints. In an agile organizations, this is bad and doesn't give fast feedback loop.

Integration tests are less expensive than manual testings, however, integration tests are expensive to maintain and are hard to run in parallel, this is due to shared state such as webserver & database state. Large number of integration tests are slow to run, testing from the browsers to the web server to the database probably can be as much as 30 stack frames deep. It is also difficult to manage state and make the tests run in parallel.