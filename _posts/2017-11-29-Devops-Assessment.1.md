---
title:  "Devops Assessment"
tags: [devops]
---

As DevOps consultant, when we have a new clients to partner with. Our approach is first to assess the devops capabilities of the clients. This help us to identify gaps, which we can then propose how we can bridge those gaps.

For example, these are some questions we ask:

## Process and Agility

The goal for modern application delivery is responsiveness, which relies on flexible scheduling, limiting work in process in favor of iterative experiments, and close team collaboration to facilitate real-time communication and eliminate wasteful handoffs. High performers have multidisciplinary feature crews who pull from a common product-backlog, minimize work in process, and deliver work ready to deploy live at the end of each sprint.

### How do you plan, prioritize and schedule work?

- In timeboxes such as sprints
- By feature completion against spec
- We strive for a one-piece flow of product backlog items, stack-ranked, and delivered continuously
- We create measurable experiments from hypotheses and use production data to assess results

### How do you manage suppliers for outsourced development?

- We negotiate fixed price and fixed date contracts
- We pay time and materials
- We use the scope of work to estimate initially and adjust the estimates accordingly
- We work on a basis of establishing trust and frequently reviewing outcomes as a basis for renewal. The supplier only produces value to the customer based on the mutually managed backlogs

### What is your Definition of Done?

- The person doing the work decides if work is complete
- Done means a potentially shippable increment is available at the end of a timebox
- Done means software is deployed in production, available to real users
- Done means we are collecting real-time telemetry on quality of service and usage against new deployments to real users

### How do development and operations teams collaborate during a production issue?

- Production incidents are handled by operations staff. Only If all operational procedures have failed to restore service level, then an escalation process may be started on an exception basis to engage developers
- Devs and Ops do a joint retrospective to review the root cause of an issue once it's resolved. They plan how to prevent a recurrence
- They collaborate in real time, in a physical team room/war room or online chat room, IRC, IM, or other messaging channel
- Developers rotate on-call duty and, when on duty, are paged to participate without delay in troubleshooting live site incidents related to their area of code

### What are your policies around open source software?

- Using open source components requires legal review and management approval
- We keep a central package store of approved open source components
- We automatically scan open source components for vulnerabilities and IP before taking a dependency on them
- Our release pipeline automatically scans OSS component versions before every deployment and we remediate any newly discovered vulnerabilities

> Top-performing organizations go beyond cross-functional to create true multi-disciplinary teams including business, dev, test, and IT ops. They allow autonomous teams to go fast, while aligning with enterprise objectives. Their quality practices are nimble and rigorous. Even across teams, clear shared ownership and a culture of user empathy make strong collaboration normative. Development activities result in measurable experiments that can inform decision-making. Here is a good starting point for Agile work practices.

## Version Control

Version control enables teams located anywhere in the world to communicate effectively during daily development activities as well as to integrate with software development tools for monitoring activities such as deployments.

### What files do you keep under version control?

- Source code and tests
- Application source code
- Configuration files
- All files used to build, test, configure, deploy, run and document our service

### What is your branching policy?

- We keep new work in separate branches and merge only when finished, usually in weeks or months
- We have no branches and develop only in the trunk or master
- We use branches that live for a few days max and commit all changes to the master or trunk at least daily
- We keep traceability on code changes but treat branches as temporary by using topic branches that we squash-merge to master when the work is complete

### How do you review code changes?

- We review code changes infrequently
- Some code is reviewed and we capture the review history using a pull request workflow or the equivalent
- We make sure all code changes are peer-reviewed before being committed to the master or trunk by means of a pull request workflow or the equivalent

### How do you manage code dependencies across teams?

- We track them in a spreadsheet or document

- We use a work item tracking system

- We keep versioned interfaces between our different services and services are automatically tested against the client contracts. Pull requests or commits of code violating the tests are rejected to prevent breaking the interface

- We maintain an artifact repository (Nuget, Nexus, Artifactory, etc.) with the latest binaries and all teams pull from there. Our services are built to cope with downstream failure and we support multiple versions

> The best performers version everything: code, tests, configurations, pipeline definitions, docs. They use "topic" branches for short-term isolation only and make sure that all code changes are merged into the common master or trunk continuously. Workflows such as Git pull requests ensure that changes are reviewed and auditable. Here is a good starting point for learning about version control with Git.

## Continuous Integration and Continuous Delivery

Continuous Integration refers to the practice of triggering an automated build and test sequence with every commit of code changes. Continuous Delivery extends this to trigger further testing and the deployment to production, with approval if necessary. CI/CD form the core DevOps flow, enabling a team to move software swiftly from initial idea through creation and validation into a production release without impediments or manual rework.

### How do you roll out new features to users?

- We deploy when the appropriate manager approves

- We run a full test suite in a production-realistic environment and then deploy

- We use progressive deployment with feature flags or exposure control and watch telemetry to determine whether to continue rollout

- All gets deployed continuously but we use feature flags to control which users have access to new features in production

> The State of DevOps Report confirms that top-performing organizations deploy 200x more frequently than low-performers. Dependable Continuous Delivery is the core capability to enable this speed. The shorter cycle times support increased responsiveness, with 24x faster recovery from failure, and higher quality, with 3x lower change failure rates. These capabilities in turn foster stakeholder satisfaction and trust. Here is a good starting point for continuous delivery.

## Cloud and Infrastructure

In the past, adding infrastructure was always bottlenecked and fixed cost to budget. Nowadays, teams can use the Public and Hybrid Clouds to gain capacity on demand. With a well-managed cloud, your teams can provision resources as needed and move as fast as they need.

### What is your policy for using the public cloud?

- We don't allow use of the public cloud

- Teams are free to decide on their own to run their virtual machines

- We allow use of the cloud (IaaS, PaaS & SaaS), as long as corporate data is secure

- We strive for the best security, end-user experience and quality of service in a cloud-native manner

### How are passwords and other secrets managed?

- Secrets are managed by Operations

- We rotate secrets when we need to and keep track of them in a versioned way

- Most secrets are kept in an encrypted vault

- We scan code and all other files to make sure no secrets are stored outside of the vault

> Public and Hybrid Clouds have made the impossible easy. The cloud has removed traditional bottlenecks and helped commoditize infrastructure. Whether you use Infrastructure as a Service (IaaS) to lift and shift your existing apps, or Platform as a Service (PaaS) to gain unprecedented productivity, or containerize your apps for high density and development efficiency, the cloud gives you a datacenter without limits. Here is a good starting point for infrastructure as code.

## Testing

Testing used to be a slow, infrequent activity. So slow, that testing cadence would determine a team's ability to release. DevOps strives for testing as a continuous activity, embedded into both the developer workflow and the pipeline used for continuous integration and continuous delivery.

### What does your team consider good testing?

- Testing that validates conformance to requirements and fixes of identified bugs

- Testing that covers most used functionality, data, code paths and configurations, with coverage measured against them

- Testing that is fast enough and thorough enough to provide feedback in the continuous integration and continuous delivery flow

- Testing that promotes continuous improvement in user satisfaction, quality of service, and performance on business objectives

### How do you test mobile apps?

- We test manually when we can

- We have automated tests to ensure new release works on different form factors

- We test against a device matrix or cloud to validate on physical devices across all the necessary form factors

- We integrate cross-device testing into our release pipeline so that every release is validated against all form factors we expect before being pushed

> High performers test continuously, running tests and validating software during coding, building and deploying the app. Continuous testing allows development teams to identify issues early and enables them to deliver a continuous stream of customer value fast and with high confidence. Without continuous testing, release cycles are long and often get delayed because critical issues are found too late in the cycle. Here is a good starting point for continuous testing.

## Monitoring

Monitoring of running applications in production environments enables a DevOps team to detect issues as they occur, to mitigate the impact, and to understand the application health. Further monitoring of customer usage helps organizations form hypotheses and quickly validate or disprove experiments.

### What instrumentation do you use to monitor applications running in production?

- Availability monitoring via pinging or synthetic transactions

- Performance monitoring of the servers and infrastructure

- Real user monitoring

- Multi-tier views of end-to-end transactions including processing and network combined

> Good decisions are informed by data. High performers instrument everything, not just for health, availability, performance, and other qualities of service, but to understand usage and to collect evidence relative to the backlog hypotheses. For example, they will experiment with changes to user experience and measure the impact on conversion rates in the funnel. They will contrast usage data among cohorts, such as weekday and weekend users, to hypothesize ways of improving the experience for each. Here is a good starting point for monitoring.

## Culture

Organizational culture matters. If team members feel that they know how to innovate and take risks, they will go farther. Leadership that supports, stimulates, inspires and recognizes individual contribution makes a measurable difference.

### Who makes decisions in the organization?

- All decisions are made by leadership/management

- We get to make decisions, but management support for the resulting issues is spotty

- Leadership has provided mechanisms by which to make low to medium-impact decisions in a safe way

- Leadership sets guardrails and communication mechanisms for safe decision-making. Guardrails are reviewed regularly to ensure they continue to foster autonomy and safety

### How does your team collaborate, share risks, innovate, and learn?

- We keep to ourselves to avoid being blamed for failure. Risk-taking is discouraged

- There is some communication. When mistakes are made, there is justice for the responsible person

- Communication and collaboration is good among teams is welcome. Innovation is encouraged

- There is high cooperation among teams. Risks are shared. Failure leads to learning and inquiry to help the team improve. Innovation is encouraged and implemented

## Measurement

Measurement is key to being able to assess performance and target improvement. Measurement allows you to see the state of the app in production, the flow from idea to code to delivery, and the actual usage of the features you produce.

### How do you measure code quality?

- Based on bugs found

- Based on code reviews

- Pre-production code profiling, static code analysis or unit testing/code coverage

- We track the density of production incidents and performance back to the code

### What decisions do you make by looking at usage data from your applications?

- Marketing campaigns track usage to understand response and conversion rates

- Feature requests and bug reports inform the teams decisions about the product backlog

- Usage telemetry informs the next steps in the product backlog

- Usage telemetry, A/B test results, and performance data are collected continuously and inform the next experiments that are run in every release

> Deployment frequency, lead time for changes, change failure rate, and time to recover are key indicators of performance. They correlate strongly with other measures of IT and business success and are prime ways to assess DevOps effectiveness. Here is a good example for the use of metrics.

## Outcomes

The point of DevOps is to achieve better outcomes. More frequent deployments allow you introduce new value more quickly. Higher deployment velocity gives you faster feedback on every change. Faster time to mitigate failures gives your users higher availability. More successful changes eliminate rework and let you go faster. All of these lead to more satisfied cusotmers and more motivated employees.

### For the application or service you work on, how often does your organization deploy code to production?

- Less often than once per six months

- Between once per month and once every 6 months

- Between once per week and once per month

- Between once per day and once per week

- Between once per hour and once per day

- On demand (multiple deploys per day)

### What is your lead time for changes (i.e., how long does it take to go from "code committed" to "code successfully running in production")?

- More than six months

- Between one month and six months

- Between one week and one month

- Between one day and one week

- Less than one day

- Less than one hour

> High performers always track the live site status, remediate any live site issues at root cause, and proactively identify any outliers in performance to see why they are experiencing slowdowns. The most effective teams see minimal mean time to repair (MTTR) and high degrees of automated response, are able to extend monitoring to real user metrics (RUM), and utilize flighting or testing in production. Here is a good starting point for DevOps outcomes.

## Conclusions
These assessments are example of gaps we can use to assess our clients. These are extracted from http://devopsassessment.net based on the DevOps Research and Assessment (DORA).