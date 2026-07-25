---
title: "Cleanup"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

Cleanup mọi short-lived resource ngay sau live demo đã được cho phép. Retained evidence không cần active compute.

## 1. Xóa API Gateway và Lambda

```bash
python lambda/deploy_api_gateway.py \
  --base-name agent-risk-score \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --region ap-southeast-1 \
  --cleanup
```

## 2. Xóa Endpoint resources

```bash
python inference/deploy_sagemaker_endpoint.py \
  --bucket "<ap-southeast-1-serving-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region ap-southeast-1 \
  --model-name agent-risk-local-xgboost \
  --cleanup
```

Helper gửi yêu cầu xóa theo dependency order: Endpoint → Endpoint Config → Model. Endpoint deletion là asynchronous. Chờ Endpoint biến mất, sau đó chạy lại cùng cleanup command một lần nếu configuration hoặc model còn dependency ở lần đầu.

## 3. Xóa monitoring UI resources

```bash
python monitoring/cloudwatch_monitoring.py \
  --base-name agent-risk-score \
  --region ap-southeast-1 \
  --cleanup

python monitoring/model_monitor.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region us-east-1 \
  --schedule-name agent-risk-model-monitor \
  --cleanup
```

## Final absence checklist

- Không còn demo Endpoint, Endpoint Config hoặc SageMaker Model.
- Không còn temporary Lambda function hoặc API Gateway HTTP API.
- Không còn Model Monitor schedule hoặc temporary monitoring Endpoint.
- Không còn CloudWatch dashboard hoặc alarms tạo cho demo.
- Không có Studio app đang chạy.
- Không có active Processing, Training, HPO hoặc Pipeline execution.
- Không lộ credentials trong logs, screenshots hoặc trajectory data.

## Giữ lại làm evidence

Chỉ giữ S3 raw/processed data, model artifacts, held-out evaluation reports, Pipeline/HPO metadata, Data Capture records, Model Monitor baseline/reports và CloudWatch logs/metrics cần thiết. Áp dụng lifecycle/log-retention policy phù hợp thay vì xóa accepted submission evidence.

Tại lần kiểm tra nghiệm thu cuối, serving/API resources, monitoring schedule, dashboard, alarms, temporary monitoring Endpoint và Studio apps đều không còn hoạt động.
