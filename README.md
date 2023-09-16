# terraform-gitlab-pipeline

## Description
- Simple Gitlab pipeline for a Terraform deployment to AWS

## Prerequisites
- The following environment variables will need to be generated:

```bash
AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
```
- These enviornment variables are credentails to use for the respective AWS account in which the the resources will be deployed
- The credentials are generated from the [AWS IAM service](https://console.aws.amazon.com/iam/)
- Instructions for credential generation are [here](https://docs.aws.amazon.com/keyspaces/latest/devguide/access.credentials.html)
- The credentials give access to permitted AWS APIs
- It is recommended that credentials for the Pipeline are for this pipeline, not a given user

## Pipeline Jobs

1. format:
        Rewrites the Terraform configuration files to a canonical format and style
2. [tflint](https://hub.docker.com/r/wata727/tflint/):
        Terraform linter for detecting errors that cannot be detected by ```terraform plan```
3. validate:
        Validates the configuration files in a directory, referring only to the configuration and not accessing any remote services such as remote state, provider APIs, etc.
4. [checkov](https://github.com/bridgecrewio/checkov):
        It scans cloud infrastructure provisioned using Terraform, Terraform plan, Cloudformation, AWS SAM, Kubernetes, Helm charts, Kustomize, Dockerfile, Serverless, Bicep, OpenAPI or ARM Templates and detects security and compliance misconfigurations using graph-based scanning.
5. plan_apply:
        Creates an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure, in this pipeline what it plans to deploy to AWS
6. apply:
        Executes the actions proposed in a Terraform plan, deploying the planned infrastructure to AWS
7. plan_destroy:
        Creates an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure, in this pipeline what it plans to destroy in AWS
8. destroy:
        Executes the actions proposed in a Terraform plan, destroying the planned infrastructure in AWS

# Manual Stages
The following stages are manual stages in the pipeline as a review of the apply and destroy plans are required before creation or destruction of resources; plans are stored off as temporary artefacts specifically for review, expiration of these artefacts can be configured as needed;  destroy_plan is manual in order to plan to destroy what is applied:  
1. apply
2. destroy_plan
3. destroy

