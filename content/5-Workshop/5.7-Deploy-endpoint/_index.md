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

![Succeeded SageMaker Pipeline execution graph](/images/5-Workshop/current/pipeline-execution-z9y3p0bqaske-succeeded-graph.png)

*Figure 1. Execution `z9y3p0bqaske` succeeded through the conditional registration graph; no deployment step is present.*

Model Package Group `agent-risk-scorer` contains versions `/1` and `/2`, both `Completed` and `PendingManualApproval`. Passing the gate permits registration only. Neither version was approved or deployed, and this workshop does not provide an approval command. A future release requires manual review, compatible serving packaging, and a separate deployment decision.

![Model Registry versions pending manual approval](/images/5-Workshop/current/model-registry-agent-risk-scorer-v1-v2-pending.png)

*Figure 2. Model package versions `/1` and `/2` are `Completed` but remain `PendingManualApproval`.*

## External/OOD Does Not Change Governance State

The 40-sample External/OOD pilot evaluated the frozen model locally. It did not start this Pipeline, register another package, approve or deploy `/1` or `/2`, tune the decision threshold, update the historical Endpoint, or replace the 854-row Model Monitor baseline. Any future retraining must begin only after representative data and independent human labels are available, then pass the same governed evaluation and manual release boundary.
