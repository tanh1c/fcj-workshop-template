---
title: "Historical Endpoint và scoring API"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

Chương này ghi lại short-lived serving demo đã nghiệm thu tại `ap-southeast-1`. Demo dùng artifact train local trước đó, không deploy Model Registry versions `/1` hoặc `/2`.

> **Cảnh báo tài nguyên trả phí:** Các command dưới đây tạo Endpoint, Lambda và HTTP API. Chỉ chạy sau khi xác nhận rõ scope, instance type, request count và cleanup owner.

## Deploy historical artifact với Data Capture

```bash
python inference/deploy_sagemaker_endpoint.py \
  --bucket "<ap-southeast-1-serving-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region ap-southeast-1 \
  --model-name agent-risk-local-xgboost \
  --instance-type ml.t2.medium \
  --capture-s3-uri "s3://<ap-southeast-1-serving-bucket>/agent-risk-scorer/data-capture/agent-risk-local-xgboost"
```

Capture URI tùy chọn bật 100% JSON input/output capture. Chờ `InService`; nếu deploy fail, cleanup phần đã tạo và dùng accepted evidence thay vì retry mù hoặc tăng instance size.

## Direct invoke

```bash
python inference/invoke_sagemaker_endpoint.py \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --region ap-southeast-1
```

## Deploy Lambda và API Gateway

```bash
python lambda/deploy_api_gateway.py \
  --base-name agent-risk-score \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --region ap-southeast-1 \
  --role-arn "<lambda-execution-role-arn>"
```

Dùng URL mới do command in ra. Historical URLs đã inactive và không được tái sử dụng.

```bash
SCORE_API_URL="<URL-printed-by-deploy-command>"
python agent/agent_runner.py \
  --task "Fix login validation bug" \
  --output runs/run_login_api.json \
  --score-api-url "$SCORE_API_URL"
```

API Gateway expose `POST /score-agent-run`. Lambda map trajectory thành 17 features dùng chung, invoke SageMaker Runtime, phát `AgentRiskScorer` EMF metrics, áp dụng hard safety rules và trả response như:

```json
{
  "risk_score": 0.6078,
  "quality_score": 0.3922,
  "predicted_label": "failed",
  "decision": "require_review"
}
```

Model score không thể override deterministic protection cho destructive commands hoặc sensitive files.
