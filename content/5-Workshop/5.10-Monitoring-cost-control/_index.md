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

![SageMaker Endpoint Data Capture JSONL retained in S3](/images/5-Workshop/current/s3-data-capture-agent-risk-local-xgboost-jsonl.png)

*Figure 1. S3 retains the captured JSONL request/response after the short-lived Endpoint was removed.*

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

![CloudWatch AgentRiskScorer custom metrics overview](/images/5-Workshop/current/cloudwatch-agentrisk-metrics-1.png)

*Figure 2. CloudWatch exposes the accepted `AgentRiskScorer` custom metric namespace.*

![CloudWatch AgentRiskScorer metric details](/images/5-Workshop/current/cloudwatch-agentrisk-metrics-2.png)

*Figure 3. Retained metric evidence covers the serving decisions and scores emitted by Lambda.*

## Model Monitor — Accepted `us-east-1` Evidence

- Baseline: 854 rows, 17 serving features, `1 x ml.m5.large`.
- One-time execution: `CompletedWithViolations`.
- Drift detected: `diff_total_lines` and `latency_total_ms` exceeded the `0.1` baseline threshold.
- Two additional type violations came from integer-like boundary values in demo traffic and remain an explicit limitation.
- S3 retains `statistics.json` and `constraint_violations.json`.
- CloudWatch received 101 endpoint data metrics.

![Completed SageMaker Model Monitor baseline job](/images/5-Workshop/current/model-monitor-baseline-1784651841-completed.png)

*Figure 4. Baseline job `agent-risk-model-monitor-baseline-1784651841` completed for the 17-feature serving dataset.*

![Completed Model Monitor processing job](/images/5-Workshop/current/model-monitor-processing-job-completed.png)

*Figure 5. The retained monitoring processing job completed; the schedule was later removed.*

![Model Monitor constraint violations report](/images/5-Workshop/current/model-monitor-constraint-violations.png)

*Figure 6. `constraint_violations.json` records drift for `diff_total_lines` and `latency_total_ms` above the `0.1` threshold, plus two documented type checks.*

Do not rerun Model Monitor merely to recreate this evidence. Data Capture plus CloudWatch is the durable monitoring path.

## External/OOD Monitoring Boundary

The External/OOD pilot produced local JSON/JSONL reports only. It created no Endpoint, Data Capture record, Model Monitor execution or baseline, CloudWatch metric, log, dashboard, alarm, or other AWS resource. Its metrics are offline diagnostic evidence and do not alter the accepted AWS monitoring state above.

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
