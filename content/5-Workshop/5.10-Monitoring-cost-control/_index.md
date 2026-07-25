---
title: "Monitoring and Cost Control"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

Monitoring was accepted across Endpoint Data Capture, Lambda EMF, SageMaker Model Monitor, and CloudWatch. These are completed evidence tracks, not future-work placeholders.

## Endpoint Data Capture — `ap-southeast-1`

The historical Endpoint sampled 100% of JSON inputs and outputs when `--capture-s3-uri` was enabled. A captured API request/response remains in S3 after the Endpoint was deleted.

## Lambda EMF and Native Metrics

Lambda emits Embedded Metric Format records under namespace `AgentRiskScorer`:

```text
Invocations
Errors
Duration
RiskScore
Decisions
```

CloudWatch also retains native Lambda and SageMaker metrics. EMF avoids a direct `PutMetricData` call from the function.

## Model Monitor — Accepted `us-east-1` Evidence

- Baseline: 854 rows, 17 serving features, `1 x ml.m5.large`.
- One-time execution: `CompletedWithViolations`.
- Drift detected: `diff_total_lines` and `latency_total_ms` exceeded the `0.1` baseline threshold.
- Two additional type violations came from integer-like boundary values in demo traffic and remain an explicit limitation.
- S3 retains `statistics.json` and `constraint_violations.json`.
- CloudWatch received 101 endpoint data metrics.

Do not rerun Model Monitor merely to recreate this evidence. Data Capture plus CloudWatch is the durable monitoring path.

## Dashboard and Alarms

The helper was accepted by creating dashboard `agent-risk-score-dashboard` and seven alarms. All alarms had `ActionsEnabled=false` and `TreatMissingData=notBreaching`; the dashboard and alarms were deleted after verification while metrics and logs remained.

```bash
python monitoring/cloudwatch_monitoring.py \
  --base-name agent-risk-score \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --function-name agent-risk-score-lambda \
  --region ap-southeast-1 \
  --cleanup
```

## Cost Controls

- Keep real-time Endpoints short-lived.
- Bound Training/HPO job count, runtime, and instance type.
- Preview HPO and Pipeline requests before explicit start flags.
- Disable alarm actions during acceptance.
- Retain artifacts and evidence instead of rerunning paid jobs.
- Verify that no Endpoint, monitoring schedule, or Studio app remains active.
