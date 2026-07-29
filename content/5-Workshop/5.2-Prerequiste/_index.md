---
title: "Prerequisites and Safety Gate"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Local Prerequisites

- Python 3.12 or a compatible Python 3 version.
- AWS CLI configured for an authorized AWS account.
- Project source code and dependencies from `requirements.txt`.

```bash
pip install -r requirements.txt
PYTHONPATH=demo_repo pytest demo_repo/tests -v
pytest agent data_generation preprocessing training inference pipelines lambda monitoring -q
```

## AWS Boundaries

| Scope | Region |
|---|---|
| Managed Training, evaluation, HPO, Pipeline, Registry, Model Monitor evidence | `us-east-1` |
| Historical Processing and optional short-lived serving/API | `ap-southeast-1` |

This boundary is quota-driven: the required SageMaker Training quota was not approved in `ap-southeast-1`, while `1 x ml.m5.large` was approved in `us-east-1`.

Use placeholders in every reusable command:

```bash
TRAINING_BUCKET="<us-east-1-training-bucket>"
SERVING_BUCKET="<ap-southeast-1-serving-bucket>"
SAGEMAKER_ROLE_ARN="<sagemaker-execution-role-arn>"
LAMBDA_ROLE_ARN="<lambda-execution-role-arn>"
```

Keep S3 Block Public Access enabled, separate the SageMaker and Lambda roles, grant least privilege, and never store or display credentials.

## Accepted Studio State

![SageMaker Studio Domain, User Profile, and Space in service](/images/5-Workshop/current/sagemaker-studio-domain-user-space-inservice.png)

*Figure 1. The Studio Domain, User Profile, and Space were all `InService` in `ap-southeast-1`.*

![SageMaker Studio with zero running applications](/images/5-Workshop/current/sagemaker-studio-zero-running-apps.png)

*Figure 2. The accepted Studio check showed zero running applications, so no notebook compute remained active.*

## Confirmation Gate for Paid Operations

Reading the workshop and inspecting retained evidence create no AWS resources. Before any paid command, explicitly confirm:

```text
Region and account: verified
Resources: exact jobs/endpoints/functions/APIs listed
Instance type and maximum job count: verified
Traffic: bounded to the stated requests
Cleanup owner and command: ready
Credentials or login details on screen: none
```

Do not rerun Processing, Training, HPO, Pipeline, Model Monitor, or an Endpoint only to recreate screenshots. If live serving is authorized, create one short-lived Endpoint, Lambda, and HTTP API in `ap-southeast-1`, send only the planned requests, and clean them up immediately.
