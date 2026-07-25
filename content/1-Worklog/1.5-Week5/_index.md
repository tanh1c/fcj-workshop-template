---
title: "Week 5: SageMaker Processing and Feature Engineering"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

## 29/06/2026 - 05/07/2026

**Work mode:** Individual implementation combined with group-based learning and discussion.
**Program:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** No fixed mentor assigned; work was self-managed and supported by documentation, tutorials, and peer discussion.

## Objective

Implement the managed processing step that transforms raw trajectory JSONL data into tabular ML features.

## Context

This was the first major AWS ML workflow step. The goal was to prove that raw logs could be processed on SageMaker infrastructure and written back to S3 as clean train/validation/test CSV files.

## AWS Learning Focus

This week focused on Amazon SageMaker Processing, the first major managed ML workflow service used in the project. I studied both the purpose of the service and the concrete operations needed to run a processing job.

- **Purpose of SageMaker Processing:** Learned that Processing is used to run data preparation, validation, transformation, and feature engineering jobs on managed infrastructure.
- **Processing script:** Studied how a Python script can receive input files, transform records, and write output files inside the processing container.
- **ProcessingInput:** Reviewed how input data from S3 is mounted into the processing job so the script can read raw JSONL files.
- **ProcessingOutput:** Reviewed how generated files are uploaded from the container back to an S3 output prefix when the job ends.
- **Processing image and instance type:** Studied that a processing job needs a container image and compute instance type, such as a CPU-based instance for tabular preprocessing.
- **IAM execution role:** Checked that SageMaker needs permission to read from the raw S3 prefix and write to the processed S3 prefix.
- **CloudWatch logs:** Reviewed logs as the main way to debug whether the processing script ran successfully.
- **Project application:** Applied these concepts by converting trajectory JSONL logs into `train.csv`, `validation.csv`, and `test.csv`.

This section shows that the processing step was not only a code transformation task; it was also an AWS managed job with S3 input/output, IAM, compute, and logs.

## Daily Breakdown

| Date | Work performed |
|---|---|
| 29/06/2026 | Implemented feature extraction logic from trajectory JSONL records into tabular fields. |
| 30/06/2026 | Tested local processing output and verified train, validation, and test CSV generation. |
| 01/07/2026 | Prepared SageMaker Processing configuration with S3 input and output paths. |
| 02/07/2026 | Ran or verified the SageMaker Processing workflow and confirmed processed output locations. |
| 03/07/2026 | Checked S3 processed CSV files and CLI/processing evidence for the completed job. |
| 04/07/2026 - 05/07/2026 | Captured S3 processed output screenshots and documented feature engineering in the workshop. |


## Technical Activities

- Implemented feature engineering logic for file counts, modified file counts, command counts, test/lint pass indicators, sensitive file flags, risky command flags, network command flags, and diff-size features.
- Converted labels into model-ready target values while keeping the original meaning documented for report explanation.
- Configured SageMaker Processing input and output paths so the processing job could read raw JSONL from S3 and write CSV outputs back to S3.
- Reviewed job status and logs through SageMaker/CloudWatch to confirm that the processing job completed and released compute resources.

## Deliverables

- **Feature engineering script completed.**
- **Train/validation/test CSV outputs generated.**
- **SageMaker Processing job verified.**
- **Processing step documented for workshop reproduction.**

## Challenge and Solution

**Challenge:** The main issue was keeping the processing output simple enough for XGBoost while still preserving important safety signals.

**Solution:** The features were kept tabular and interpretable so the workshop can explain why a risky run receives a higher risk score.

## Project Relevance

This week contributed to the final MVP by strengthening the path from **AI coding-agent behavior evidence** to an **AWS-based risk scoring workflow**. The work helped ensure that the final workshop is not only a conceptual explanation, but also follows the actual implementation sequence used in the project.

## Evidence Screenshots

![Processed CSV outputs in S3](/images/worklog/week05-processed-csv-s3.png)

![SageMaker Processing job CLI evidence](/images/worklog/week05-sagemaker-processing-job-cli.png)

The screenshots show that the processing step produced train, validation, and test CSV files under the project S3 prefix.

## Evidence and References Studied

- [SageMaker Processing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)
- [SageMaker Python SDK Processing API](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_processing.html)
- [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)

---

[Previous](/1-worklog/1.4-week4/) | [Back to Worklog](/1-worklog/) | [Next](/1-worklog/1.6-week6/)
