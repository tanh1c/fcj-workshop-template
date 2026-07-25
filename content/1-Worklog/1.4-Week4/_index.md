---
title: "Week 4: S3 Data Layout and IAM Preparation"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

## 22/06/2026 - 28/06/2026

**Work mode:** Individual implementation combined with group-based learning and discussion.
**Program:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** No fixed mentor assigned; work was self-managed and supported by documentation, tutorials, and peer discussion.

## Objective

Prepare the AWS storage and permission foundation needed to move data and artifacts through the MVP workflow safely.

## Context

Before running SageMaker jobs, the project needed a clear S3 layout and IAM plan. This reduced confusion when raw logs, processed CSV files, and model artifacts moved between local development, SageMaker, Lambda, and the endpoint.

## AWS Learning Focus

This week focused on Amazon S3 and IAM because they are the foundation for most later AWS services in the project. Before running SageMaker Processing, I needed to understand how data paths and permissions should be organized.

- **Amazon S3 bucket usage:** Studied how one bucket can store multiple project areas using prefixes instead of separate physical folders.
- **S3 prefix design:** Planned the project layout with `raw/` for JSONL logs, `processed/` for CSV outputs, and `models/` for model artifacts.
- **AWS CLI operations:** Practiced or reviewed commands for listing buckets, uploading files, checking object paths, and verifying whether expected outputs exist in S3.
- **S3 URI format:** Learned how paths such as `s3://bucket/prefix/file` are passed into SageMaker jobs and deployment scripts.
- **IAM execution role:** Studied why SageMaker needs an execution role to read input data from S3 and write output data back.
- **Least-privilege access:** Reviewed why roles should only receive the permissions needed for the workflow, rather than broad administrator access.
- **Evidence collection:** Used S3 screenshots and object paths as durable evidence because S3 artifacts can remain after temporary compute resources are cleaned up.

This AWS learning made the project workflow more repeatable because all later processing, training, and deployment steps depended on stable S3 paths and correct IAM permissions.

## Daily Breakdown

| Date | Work performed |
|---|---|
| 22/06/2026 | Planned the S3 prefix structure for raw logs, processed datasets, and model artifacts. |
| 23/06/2026 | Created or verified the project S3 bucket and checked the expected folder layout. |
| 24/06/2026 | Reviewed SageMaker execution role requirements for reading input data and writing processed outputs. |
| 25/06/2026 | Reviewed Lambda role requirements for CloudWatch logging and SageMaker Runtime invocation. |
| 26/06/2026 | Prepared AWS CLI commands for checking S3 paths and repeatable evidence collection. |
| 27/06/2026 - 28/06/2026 | Captured S3 and IAM screenshots and documented the resource naming convention. |


## Technical Activities

- Planned S3 prefixes for raw trajectory logs, processed train/validation/test datasets, and trained model artifacts.
- Reviewed least-privilege access patterns for SageMaker execution roles, Lambda execution roles, S3 read/write permissions, and SageMaker Runtime invocation.
- Prepared AWS CLI commands for uploading raw JSONL logs, listing generated outputs, and checking artifact locations.
- Documented environment details used later in the report, including region, account-scoped bucket naming, role naming, and resource cleanup expectations.

## Deliverables

- **S3 data layout prepared.**
- **IAM role responsibilities clarified.**
- **AWS CLI workflow drafted.**
- **Resource naming convention documented.**

## Challenge and Solution

**Challenge:** IAM can easily become either too permissive or too restrictive during early demos.

**Solution:** The permissions were reasoned service by service: SageMaker needed S3 access for processing and model artifacts, Lambda needed SageMaker Runtime access, and CloudWatch logging was required for debugging.

## Project Relevance

This week contributed to the final MVP by strengthening the path from **AI coding-agent behavior evidence** to an **AWS-based risk scoring workflow**. The work helped ensure that the final workshop is not only a conceptual explanation, but also follows the actual implementation sequence used in the project.

## Evidence Screenshots

![S3 bucket layout for project data](/images/worklog/week04-s3-layout.png)

![IAM role used by the AWS workflow](/images/worklog/week04-iam-role.png)

The screenshots show the storage and permission foundation used before running the AWS-side processing workflow.

## Evidence and References Studied

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)

---

[Previous](/1-worklog/1.3-week3/) | [Back to Worklog](/1-worklog/) | [Next](/1-worklog/1.5-week5/)
