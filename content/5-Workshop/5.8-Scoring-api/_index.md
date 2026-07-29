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

![Accepted API score response requiring human review](/images/5-Workshop/current/local-evidence-api-score-response-require-review.png)

*Figure 1. The retained scoring response returns `require_review`; the historical API URL itself is not published.*

A model score cannot override deterministic protection for destructive commands or sensitive files.

## Local Negative Safety Validation

The corresponding rule layer was validated locally without creating an AWS resource or calling SageMaker. From the source repository root, run:

```bash
PYTHONPATH=. python -m pytest -v \
  inference/test_decision_policy.py::test_decide_hard_blocks_destructive_command \
  inference/test_decision_policy.py::test_decide_requires_review_for_sensitive_file_even_when_model_low_risk \
  agent/test_tool_policy.py::test_path_policy_blocks_sensitive_paths
```

Result: `3 passed in 0.05s`.

| Negative input | Expected result | Observed result |
|---|---|---|
| `destructive_command_detected=true` with model probabilities set to `safe=1.0` | Decision `block`; reason identifies the destructive command. | `block`; reason `Destructive command detected`. |
| `touched_sensitive_files=true` with model probabilities set to `safe=1.0` | Decision `require_review`; reason identifies the sensitive file. | `require_review`; reason `Sensitive file touched`. |
| `demo_repo/.env`, `.github/workflows/deploy.yml`, or `secrets/config.py` | Path policy rejects each sensitive path. | All three returned `false` from `is_path_allowed`. |

These are local rule and policy tests, not a claim that the historical API returned a particular HTTP error for malformed payloads. No API error response was recorded for this acceptance run.
