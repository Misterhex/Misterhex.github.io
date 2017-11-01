---
tags: [devops, testing]
---

Recently, I read this [article](https://watirmelon.blog/2012/01/31/introducing-the-software-testing-ice-cream-cone/) on integration tests anti pattern and it really reminds me of one of our client. This is a dreadful situation with my consulting client, they have a culture that falls exactly into this anti pattern known as the testing ice cream cone.

![cname file content]({{ site.url }}/assets/img/softwaretestingicecreamconeantipattern.png)

As illustrated in the diagram above, from top to bottom, this organization spend majority of its resources & efforts on manual testings and least on unit tests. This is a very common pattern i have seen throughout our career. 

### Why is this pattern bad?

#### Manual Testing
Heavily focus and reliance on manual testing is a slow, non-scalable, non-repeatable and tedious process. Manual testers have to repeatly check features that are developed in this sprint and also in the last N sprints. This is a very old school style of leveraging cheap army of human labour that mindlessly check for software defects. Also, these work can't be automated as well. ( of course! ) Manual testing is an easy answers to getting low defect but only in the initial stage of the products because it can't scale in term of cost, accuracy and complexity.

#### Integration Testing
Integration tests are less expensive than manual testings, however, integration tests are expensive to maintain and are hard to run in parallel, this is due to shared state such as webserver & database state. Large number of integration tests are slow to run, testing from the browsers to the web server to the database probably can be as much as 30 stack frames deep. It is also difficult to manage state and make the tests run in parallel.

### Test Pyramid
The recommended approach is illustrated by the diagram below. The organization invested majority of tests in unit tests, that are completely isolated, provide instant feedback and has no problem running in full parallel.

![cname file content]({{ site.url }}/assets/img/idealautomatedtestingpyramid.png)

### Conclusion
In conclusion, focus effort on unit testing, have some integration and end-to-end tests and very little manual testers to strike for a good balance.