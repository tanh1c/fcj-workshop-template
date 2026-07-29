---
title: "Architecture Step 2: Governance, Inference, and Monitoring"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.3.2. </b> "
---

## Governed Registration — `us-east-1`

```text
Preprocess
  -> Train
  -> Evaluate
  -> CheckRiskyRecall
       -> RegisterModel when risky_recall >= 0.85
       -> Stop without registration otherwise
```

A passing metric enables registration only. Packages `/1` and `/2` remain `Completed` and `PendingManualApproval`. Approval and deployment require a separate manual release decision; the Pipeline contains no Endpoint step.

## Historical Serving — `ap-southeast-1`

```text
Client POST /score-agent-run
  -> API Gateway
  -> Lambda: trajectory to 17 features
  -> historical SageMaker Endpoint
  -> Lambda
  -> API Gateway
  -> Client response
```

This short-lived Endpoint used the earlier local artifact, not a Registry package.

## Monitoring Paths

```text
Endpoint -> S3 Data Capture -> Model Monitor -> reports and data metrics
Endpoint -> CloudWatch native SageMaker metrics
Lambda   -> CloudWatch native metrics + AgentRiskScorer EMF metrics
```

Data Capture sampled 100% of JSON input/output during acceptance. Serving resources, the monitoring schedule, dashboard, and alarms were cleaned up; retained S3 records, reports, logs, and metrics remain evidence.

## External/OOD Boundary

The External/OOD pilot is a third, local-only evidence path. It reused the frozen 17-feature model but did not modify the Pipeline, Model Registry packages, historical Endpoint, threshold, or Model Monitor baseline. Its low external scores diagnose distribution shift; they do not authorize a release or replace reviewer judgment.
