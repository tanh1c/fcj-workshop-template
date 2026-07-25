---
title: "Pipeline and Model Registry Governance"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

The SageMaker Pipeline automates model creation and conditional registration in `us-east-1`. It intentionally contains no Endpoint deployment step.

## Inspect the Pipeline Locally

The default command compiles and prints the Pipeline definition without upserting or starting it:

```bash
python pipelines/sagemaker_pipeline.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region us-east-1
```

Review its parameters and graph:

```text
Preprocess
  -> Train
  -> Evaluate
  -> CheckRiskyRecall
       -> RegisterModel when risky_recall >= 0.85
       -> Stop without registration otherwise
```

The Pipeline uploads five code assets only when explicitly published: `processing_script.py`, `train_xgboost.py`, `evaluate_model.py`, `inference.py`, and `decision_policy.py`.

## Explicit AWS Mutation

`--upsert` creates or updates the Pipeline; `--start` creates a paid execution and is rejected unless `--upsert` is also present. Do not run this merely for evidence.

```bash
python pipelines/sagemaker_pipeline.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region us-east-1 \
  --upsert \
  --start
```

## Accepted Governance Evidence

Execution `z9y3p0bqaske` succeeded through Processing, Training, held-out Evaluation, `CheckRiskyRecall`, and `RegisterModel`. Observed risky recall was `1.00` against threshold `0.85`.

Model Package Group `agent-risk-scorer` contains versions `/1` and `/2`, both `Completed` and `PendingManualApproval`. Passing the gate permits registration only. Neither version was approved or deployed, and this workshop does not provide an approval command. A future release requires manual review, compatible serving packaging, and a separate deployment decision.
