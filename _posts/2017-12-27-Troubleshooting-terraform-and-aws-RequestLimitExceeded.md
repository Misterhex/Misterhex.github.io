---
title:  "Troubleshooting Terraform And AWS RequestLimitExceeded"
---

Recently, when running terraform apply, my script has been failing intermittently when provisioning around 90 ec2 instances. 

Luckily, terraform provide debugging output by passing [TF_LOG=debug](https://www.terraform.io/docs/internals/debugging.html). It is interesting that terraform uses [aws-sdk-go](https://github.com/aws/aws-sdk-go) under the hood to issues API calls to AWS.

The root cause of my script failing was due to raising the [parallelism count](https://www.terraform.io/docs/commands/apply.html#parallelism-n) beyond the default ( 10 ). When tracing the debug logs, i noticed a lot of API requests to AWS that terraform made has failed with [RequestLimitExceeded](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/query-api-troubleshooting.html#api-request-rate), after setting back to default, i am able to consistently provision `200` ec2 instances without failure.

There are more discussion about this issue on github [here](https://github.com/hashicorp/terraform/issues/3579).