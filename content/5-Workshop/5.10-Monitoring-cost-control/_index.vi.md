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

## Model Monitor — Accepted evidence tại `us-east-1`

- Baseline: 854 rows, 17 serving features, `1 x ml.m5.large`.
- One-time execution: `CompletedWithViolations`.
- Drift detected: `diff_total_lines` và `latency_total_ms` vượt baseline threshold `0.1`.
- Hai type violations bổ sung do integer-like boundary values trong demo traffic và được giữ như limitation rõ ràng.
- S3 giữ `statistics.json` và `constraint_violations.json`.
- CloudWatch nhận 101 endpoint data metrics.

Không chạy lại Model Monitor chỉ để tái tạo evidence. Data Capture cộng CloudWatch là durable monitoring path.

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
