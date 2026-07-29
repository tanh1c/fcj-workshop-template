---
title: "Run SageMaker Processing"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

SageMaker Processing applies the same feature extraction used in the local preview and writes `train.csv`, `validation.csv`, and `test.csv` to S3.

## Free Local Preview

```bash
python preprocessing/processing_script.py \
  --input data_generation/combined_trajectories.jsonl \
  --output-dir data/processed \
  --seed 42
```

Inspect the three CSV files and their ordered columns before any AWS job.

## Optional Managed Processing

This command creates a paid Processing Job. Run it only after the confirmation gate in [Prerequisites](/5-workshop/5.2-prerequiste/).

```bash
python preprocessing/run_sagemaker_processing.py \
  --bucket "<processing-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region ap-southeast-1 \
  --instance-type ml.t3.medium \
  --input data_generation/combined_trajectories.jsonl
```

## Accepted Evidence

Historical job `agent-risk-processing-1782829845` completed in `ap-southeast-1` and produced the three CSV splits under its processed S3 prefix. No Processing Job is currently active. The Pipeline later repeated preprocessing in `us-east-1` as part of the governed execution.

![Completed SageMaker Processing Job overview](/images/5-Workshop/current/processing-job-agent-risk-processing-1782829845-completed-1.png)

*Figure 1. Processing Job `agent-risk-processing-1782829845` reached `Completed`.*

![Completed SageMaker Processing Job details](/images/5-Workshop/current/processing-job-agent-risk-processing-1782829845-completed-2.png)

*Figure 2. The retained job details identify the accepted managed Processing execution.*

![Processed train, validation, and test CSV files in S3](/images/5-Workshop/current/s3-processed-csv-splits-agent-risk-processing-1782829845.png)

*Figure 3. S3 retains the generated `train.csv`, `validation.csv`, and `test.csv` splits.*

Do not rerun Processing merely to reproduce evidence; inspect the retained CSV artifacts and accepted job record instead.
