---
title: "Kiến trúc bước 2: Governance, inference và monitoring"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

## Registration có governance — `us-east-1`

```text
Preprocess
  -> Train
  -> Evaluate
  -> CheckRiskyRecall
       -> RegisterModel khi risky_recall >= 0.85
       -> Dừng không registration nếu không đạt
```

Metric đạt gate chỉ cho phép registration. Packages `/1` và `/2` vẫn `Completed` và `PendingManualApproval`. Approval và deployment cần release decision thủ công riêng; Pipeline không có Endpoint step.

## Historical serving — `ap-southeast-1`

```text
Client POST /score-agent-run
  -> API Gateway
  -> Lambda: trajectory thành 17 features
  -> historical SageMaker Endpoint
  -> Lambda
  -> API Gateway
  -> Client response
```

Endpoint ngắn hạn này dùng artifact train local trước đó, không dùng Registry package.

## Monitoring paths

```text
Endpoint -> S3 Data Capture -> Model Monitor -> reports và data metrics
Endpoint -> CloudWatch native SageMaker metrics
Lambda   -> CloudWatch native metrics + AgentRiskScorer EMF metrics
```

Data Capture lấy mẫu 100% JSON input/output trong acceptance. Serving resources, monitoring schedule, dashboard và alarms đã cleanup; S3 records, reports, logs và metrics được giữ làm evidence.
