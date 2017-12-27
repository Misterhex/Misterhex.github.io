---
title:  "Troubleshooting Terraform And AWS RequestLimitExceeded"
---

Recently, when running terraform apply, my script has been failing intermittently when provisioning around 90 ec2 instances. 

Luckily, terraform provide debugging output by passing [TF_LOG=debug](https://www.terraform.io/docs/internals/debugging.html). It is interesting that terraform uses [aws-sdk-go](https://github.com/aws/aws-sdk-go) under the hood to issues API calls to AWS from `go` code.

The root cause of my script failing was due to raising the [parallism](https://www.terraform.io/docs/commands/apply.html#parallelism-n) counter beyond the default ( 10 ). When tracing the debug logs, i noticed a lot of API requests to AWS that terraform made has failed with `RequestLimitExceeded`, after setting back to default, i am able to consistently provision `200` ec2 instances without failure.

There are more discussion about this issue on github [here](https://github.com/hashicorp/terraform/issues/3579).

