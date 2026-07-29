---
title: "Monitoring và kiểm soát chi phí"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 5.10. </b> "
---

Monitoring đã được nghiệm thu qua Endpoint Data Capture, Lambda EMF, SageMaker Model Monitor và CloudWatch. Đây là completed evidence tracks, không phải future-work placeholders.

## Endpoint Data Capture — `ap-southeast-1`

Historical Endpoint lấy mẫu 100% JSON inputs/outputs khi bật `--capture-s3-uri`. Một captured API request/response vẫn còn trong S3 sau khi Endpoint bị xóa.

![SageMaker Endpoint Data Capture JSONL được giữ trên S3](/images/5-Workshop/current/s3-data-capture-agent-risk-local-xgboost-jsonl.png)

*Hình 1. S3 giữ captured JSONL request/response sau khi short-lived Endpoint đã bị xóa.*

## Lambda EMF và native metrics

Lambda phát Embedded Metric Format records trong namespace `AgentRiskScorer`:

```text
Invocations
Errors
Duration
RiskScore
Decisions
```

CloudWatch cũng giữ native Lambda và SageMaker metrics. EMF tránh gọi trực tiếp `PutMetricData` từ function.

![Tổng quan CloudWatch custom metrics AgentRiskScorer](/images/5-Workshop/current/cloudwatch-agentrisk-metrics-1.png)

*Hình 2. CloudWatch hiển thị custom metric namespace `AgentRiskScorer` đã nghiệm thu.*

![Chi tiết CloudWatch metrics AgentRiskScorer](/images/5-Workshop/current/cloudwatch-agentrisk-metrics-2.png)

*Hình 3. Metric evidence được giữ lại bao gồm serving decisions và scores do Lambda phát.*

## Model Monitor — Accepted evidence tại `us-east-1`

- Baseline: 854 rows, 17 serving features, `1 x ml.m5.large`.
- One-time execution: `CompletedWithViolations`.
- Drift detected: `diff_total_lines` và `latency_total_ms` vượt baseline threshold `0.1`.
- Hai type violations bổ sung do integer-like boundary values trong demo traffic và được giữ như limitation rõ ràng.
- S3 giữ `statistics.json` và `constraint_violations.json`.
- CloudWatch nhận 101 endpoint data metrics.

![SageMaker Model Monitor baseline job đã hoàn tất](/images/5-Workshop/current/model-monitor-baseline-1784651841-completed.png)

*Hình 4. Baseline job `agent-risk-model-monitor-baseline-1784651841` hoàn tất cho serving dataset gồm 17 features.*

![Model Monitor processing job đã hoàn tất](/images/5-Workshop/current/model-monitor-processing-job-completed.png)

*Hình 5. Monitoring processing job được giữ lại đã hoàn tất; schedule sau đó đã bị xóa.*

![Báo cáo Model Monitor constraint violations](/images/5-Workshop/current/model-monitor-constraint-violations.png)

*Hình 6. `constraint_violations.json` ghi drift của `diff_total_lines` và `latency_total_ms` vượt threshold `0.1`, cùng hai type checks đã được giải thích.*

Không chạy lại Model Monitor chỉ để tái tạo evidence. Data Capture cộng CloudWatch là durable monitoring path.

## Ranh giới monitoring của External/OOD

External/OOD pilot chỉ tạo local JSON/JSONL reports. Pilot không tạo Endpoint, Data Capture record, Model Monitor execution hoặc baseline, CloudWatch metric, log, dashboard, alarm hay AWS resource nào khác. Các metrics của pilot là offline diagnostic evidence và không thay đổi accepted AWS monitoring state ở trên.

## Dashboard và alarms

Helper đã được nghiệm thu bằng dashboard `agent-risk-score-dashboard` và bảy alarms. Tất cả alarms có `ActionsEnabled=false` và `TreatMissingData=notBreaching`; dashboard/alarms đã bị xóa sau verification trong khi metrics/logs được giữ.

```bash
python monitoring/cloudwatch_monitoring.py \
  --base-name agent-risk-score \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --function-name agent-risk-score-lambda \
  --region ap-southeast-1 \
  --cleanup
```

## Cost controls

- Giữ real-time Endpoints ngắn hạn.
- Giới hạn Training/HPO job count, runtime và instance type.
- Preview HPO/Pipeline requests trước explicit start flags.
- Tắt alarm actions trong acceptance.
- Giữ artifacts/evidence thay vì rerun paid jobs.
- Xác minh không còn Endpoint, monitoring schedule hoặc Studio app hoạt động.
