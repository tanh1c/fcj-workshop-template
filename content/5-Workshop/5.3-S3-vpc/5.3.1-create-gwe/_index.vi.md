---
title: "Kiến trúc bước 1: Luồng dữ liệu và managed ML"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.3.1. </b> "
---

![Vòng đời dữ liệu và ML có governance cho AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-ml-flow.webp)

*Figure 2. Trajectory evidence trở thành feature contract dùng chung, managed XGBoost artifact, held-out metrics và package được đăng ký có điều kiện.*

## Data path

```text
Simulator + SWE-bench Lite adapter
  -> labeled trajectory JSONL
  -> Amazon S3 raw data
  -> SageMaker Processing
  -> train / validation / test CSV
  -> shared 17-feature contract

Mini LLM Agent
  -> unlabeled trajectory JSON cho demo scoring
```

Processing, Training, Lambda inference và Model Monitor dùng cùng thứ tự features để tránh training-serving schema drift.

## Managed ML path — `us-east-1`

```text
Processed CSV
  -> SageMaker XGBoost 1.7-1 Training
  -> model.tar.gz trong S3
  -> held-out evaluation
  -> safety metric cho Pipeline gate
```

SageMaker Experiments và bounded Random HPO giữ trial/hyperparameter evidence hỗ trợ. HPO không thay thế held-out evaluation cung cấp metric cho `CheckRiskyRecall`.

Evidence đã nghiệm thu gồm Training Job `1 x ml.m5.large` hoàn tất, report trên 183 held-out rows và ba HPO child jobs chạy tuần tự hoàn tất. Perfect synthetic metrics chỉ chứng minh workflow execution, không chứng minh real-world generalization.

## Nhánh External/OOD local độc lập

```text
Public trajectories được pin revision (20 + 20, seed 42)
  -> annotation A/B AI-assisted độc lập
  -> adjudication các bất đồng
  -> parser 17 features dùng chung
  -> frozen managed model artifact
  -> local metrics và diagnostic evidence đã redacted
```

Hai nguồn được pin tại revision bất biến: `nebius/SWE-agent-trajectories` ở `68195a1450865274106246d0d0296a1d6807b88e` và `nebius/SWE-rebench-openhands-trajectories` ở `35455389ab51bf5e2306bfd436ef72d0f98bf882`. Diagnostic này không retrain model, tune threshold, gọi SageMaker hoặc đưa external data qua AWS Pipeline.
