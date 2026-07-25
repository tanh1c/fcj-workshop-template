---
title: "Architecture Step 1: Data and Managed ML Flow"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

![Completed governed data and ML lifecycle for AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-ml-flow.webp)

*Figure 2. Trajectory evidence becomes a shared feature contract, a managed XGBoost artifact, held-out metrics, and a conditionally registered package.*

## Data Path

```text
Simulator + SWE-bench Lite adapter
  -> labeled trajectory JSONL
  -> Amazon S3 raw data
  -> SageMaker Processing
  -> train / validation / test CSV
  -> shared 17-feature contract

Mini LLM Agent
  -> unlabeled trajectory JSON for demo scoring
```

The same ordered features are used by Processing, Training, Lambda inference, and Model Monitor. This prevents training-serving schema drift.

## Managed ML Path — `us-east-1`

```text
Processed CSV
  -> SageMaker XGBoost 1.7-1 Training
  -> model.tar.gz in S3
  -> held-out evaluation
  -> safety metric for the Pipeline gate
```

SageMaker Experiments and bounded Random HPO retain supporting trial and hyperparameter evidence. HPO does not replace the held-out evaluation that feeds `CheckRiskyRecall`.

Accepted evidence includes a completed `1 x ml.m5.large` Training Job, a report over 183 held-out rows, and three completed serial HPO child jobs. The perfect synthetic metrics prove workflow execution only, not real-world generalization.
