---
title: "Managed Training, Evaluation, and HPO"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Managed XGBoost Training, held-out evaluation, Experiments, and bounded HPO run in `us-east-1`. The commands below are reproducible shapes, not instructions to recreate accepted evidence.

## Managed XGBoost Training

This command creates a paid `ml.m5.large` Training Job. Run it only with explicit authorization.

```bash
python training/run_sagemaker_xgboost_training.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1 \
  --instance-type ml.m5.large
```

The launcher uses SageMaker XGBoost `1.7-1`, uploads the training and inference code, and writes `model.tar.gz` to S3.

## Held-Out Evaluation

Evaluate a completed managed artifact against its untouched `test.csv` and upload the report:

```bash
python training/run_managed_model_evaluation.py \
  --bucket "<us-east-1-training-bucket>" \
  --job-name "<completed-training-job-name>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1
```

The report includes accuracy, macro F1, risky recall, risky false-negative rate, per-class results, and a confusion matrix.

## Bounded Random HPO

Without `--start`, the launcher only prints the bounded request. Adding `--start` creates three serial paid child jobs.

```bash
python training/run_sagemaker_hpo.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1 \
  --instance-type ml.m5.large
```

## Accepted Evidence

- Training Job `agent-risk-xgboost-1784625353`: `Completed`, `1 x ml.m5.large`, 140 training and billable seconds.
- Held-out evaluation: 183 test rows, macro F1 `1.00`, risky recall `1.00`, risky false-negative rate `0.00`.
- HPO job `agent-risk-hpo-1784643415`: Random strategy, three completed serial child jobs, Experiment `agent-risk-scoring-experiment`.
- Selected child: `agent-risk-hpo-1784643415-001-59146c4e`.

These perfect scores come from mostly synthetic, intentionally separable labels. They validate managed workflow execution, not production quality or generalization. The earlier local model remains relevant only as the artifact used by the historical serving demo.
