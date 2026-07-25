---
title: "Week 2: Problem Definition and Project Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

## 08/06/2026 - 14/06/2026

**Work mode:** Individual implementation combined with group-based learning and discussion.
**Program:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** No fixed mentor assigned; work was self-managed and supported by documentation, tutorials, and peer discussion.

## Objective

Convert the selected topic into a concrete MVP proposal with problem statement, scope, workflow, expected API response, and AWS service responsibilities.

## Context

This week focused on making the project measurable. Instead of saying only that an AI agent is safe or unsafe, the proposal needed to define what data would be observed and what decision the system would return.

## AWS Learning Focus

This week focused on understanding how an AWS-based machine learning project should be designed before writing the proposal. I studied the purpose of each service and how they work together in a data-to-inference workflow.

- **Amazon S3:** Studied how S3 acts as the durable storage layer for raw input data, processed datasets, model artifacts, and demo evidence.
- **Amazon SageMaker Processing:** Reviewed its purpose as a managed environment for running data preprocessing and feature engineering scripts.
- **SageMaker Training:** Studied the intended managed training workflow, including training containers, input channels, output artifacts, instance types, and quotas.
- **SageMaker Endpoint:** Learned that endpoints are used for real-time inference and require a SageMaker Model plus Endpoint Configuration.
- **AWS Lambda:** Studied Lambda as a serverless compute layer that can transform API requests and call SageMaker Runtime.
- **Amazon API Gateway:** Reviewed how an HTTP API can expose a clean public route such as `POST /score-agent-run`.
- **CloudWatch and IAM:** Studied their supporting roles: CloudWatch for logs and IAM for least-privilege access between services.

This AWS learning was then converted into the project proposal architecture, where each AWS service had a clear responsibility instead of being listed only as a technology name.

## Daily Breakdown

| Date | Work performed |
|---|---|
| 08/06/2026 | Defined the project problem: evaluating AI coding-agent runs using trajectory evidence rather than final summaries only. |
| 09/06/2026 | Designed the expected input data fields for files, commands, tests, lint checks, sensitive access, and risky behavior. |
| 10/06/2026 | Drafted the scoring response fields: risk_score, quality_score, predicted_label, and decision. |
| 11/06/2026 | Mapped project stages to AWS services and separated implemented MVP scope from future MLOps extensions. |
| 12/06/2026 | Reviewed SageMaker Processing, S3, Lambda, and API Gateway documentation to validate the proposed architecture. |
| 13/06/2026 - 14/06/2026 | Refined the proposal evidence, captured proposal screenshots, and prepared the Week 2 worklog evidence. |


## Technical Activities

- Defined trajectory logs as the core evidence source, covering files read, files modified, commands executed, test results, lint results, diff size, sensitive file access, and risky command signals.
- Designed the response schema with risk_score, quality_score, predicted_label, and decision so the output can support allow, require_review, or block decisions.
- Mapped each project stage to an AWS service: S3 for raw/processed data and model artifacts, SageMaker Processing for feature engineering, SageMaker Endpoint for inference, Lambda for request handling, API Gateway for the HTTP route, CloudWatch for logs, and IAM for access control.
- Defined the honest limitation that SageMaker Training may be attempted, but local XGBoost training is acceptable if account quota blocks managed training.

## Deliverables

- **Problem statement completed.**
- **MVP scope written.**
- **API request/response idea drafted.**
- **AWS architecture responsibilities defined.**

## Challenge and Solution

**Challenge:** The risk scoring problem could become too broad if it included full production MLOps, policy engines, and real-time monitoring from the start.

**Solution:** The proposal limited the first version to a practical end-to-end path from logs to scoring API, while listing Model Registry, Model Monitor, and Pipelines as future work.

## Project Relevance

This week contributed to the final MVP by strengthening the path from **AI coding-agent behavior evidence** to an **AWS-based risk scoring workflow**. The work helped ensure that the final workshop is not only a conceptual explanation, but also follows the actual implementation sequence used in the project.

## Evidence Screenshots

![Project proposal part 1](/images/worklog/week02-project-proposal_1.png)

![Project proposal part 2](/images/worklog/week02-project-proposal_2.png)

Because the proposal content is long, the full Markdown evidence is also attached here: [week02-project-proposal.md](/images/worklog/week02-project-proposal.md).

## Evidence and References Studied

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [SageMaker Processing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)
- [Amazon API Gateway Developer Guide](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

---

[Previous](/1-worklog/1.1-week1/) | [Back to Worklog](/1-worklog/) | [Next](/1-worklog/1.3-week3/)
