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

![Response chấm điểm đã nghiệm thu yêu cầu con người review](/images/5-Workshop/current/local-evidence-api-score-response-require-review.png)

*Hình 1. Scoring response được giữ lại trả về `require_review`; historical API URL không được công bố.*

Model score không thể override deterministic protection cho destructive commands hoặc sensitive files.

## Local negative safety validation

Rule layer tương ứng đã được validate local mà không tạo AWS resource hoặc gọi SageMaker. Chạy command sau từ source repository root:

```bash
PYTHONPATH=. python -m pytest -v \
  inference/test_decision_policy.py::test_decide_hard_blocks_destructive_command \
  inference/test_decision_policy.py::test_decide_requires_review_for_sensitive_file_even_when_model_low_risk \
  agent/test_tool_policy.py::test_path_policy_blocks_sensitive_paths
```

Kết quả: `3 passed in 0.05s`.

| Negative input | Expected result | Kết quả quan sát được |
|---|---|---|
| `destructive_command_detected=true` với model probabilities đặt `safe=1.0` | Decision `block`; reason phải chỉ ra destructive command. | `block`; reason là `Destructive command detected`. |
| `touched_sensitive_files=true` với model probabilities đặt `safe=1.0` | Decision `require_review`; reason phải chỉ ra sensitive file. | `require_review`; reason là `Sensitive file touched`. |
| `demo_repo/.env`, `.github/workflows/deploy.yml` hoặc `secrets/config.py` | Path policy từ chối từng sensitive path. | Cả ba đều trả về `false` từ `is_path_allowed`. |

Đây là các local rule và policy tests, không phải tuyên bố rằng historical API đã trả về một HTTP error cụ thể cho malformed payload. Acceptance run không có API error response được ghi nhận.