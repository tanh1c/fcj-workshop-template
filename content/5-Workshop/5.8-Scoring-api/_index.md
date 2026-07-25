---
title: "Historical Endpoint and Scoring API"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 5.8. </b> "
---

This chapter documents the accepted short-lived serving demo in `ap-southeast-1`. It used the earlier locally trained artifact. It did not deploy Model Registry versions `/1` or `/2`.

> **Paid-resource warning:** The commands below create an Endpoint, Lambda, and HTTP API. Run them only after explicit confirmation of scope, instance type, request count, and cleanup ownership.

## Deploy the Historical Artifact with Data Capture

```bash
python inference/deploy_sagemaker_endpoint.py \
  --bucket "<ap-southeast-1-serving-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region ap-southeast-1 \
  --model-name agent-risk-local-xgboost \
  --instance-type ml.t2.medium \
  --capture-s3-uri "s3://<ap-southeast-1-serving-bucket>/agent-risk-scorer/data-capture/agent-risk-local-xgboost"
```

The optional capture URI enables 100% JSON input/output capture. Wait for `InService`; if deployment fails, clean up what was created and use accepted evidence rather than retrying blindly or increasing instance size.

## Direct Invoke

```bash
python inference/invoke_sagemaker_endpoint.py \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --region ap-southeast-1
```

## Deploy Lambda and API Gateway

```bash
python lambda/deploy_api_gateway.py \
  --base-name agent-risk-score \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --region ap-southeast-1 \
  --role-arn "<lambda-execution-role-arn>"
```

Use the new URL printed by the command. Historical URLs are inactive and must not be reused.

```bash
SCORE_API_URL="<URL-printed-by-deploy-command>"
python agent/agent_runner.py \
  --task "Fix login validation bug" \
  --output runs/run_login_api.json \
  --score-api-url "$SCORE_API_URL"
```

API Gateway exposes `POST /score-agent-run`. Lambda maps the trajectory into the shared 17 features, invokes SageMaker Runtime, emits `AgentRiskScorer` EMF metrics, applies hard safety rules, and returns a response such as:

```json
{
  "risk_score": 0.6078,
  "quality_score": 0.3922,
  "predicted_label": "failed",
  "decision": "require_review"
}
```

A model score cannot override deterministic protection for destructive commands or sensitive files.
