---
title: "Cleanup"
date: 2024-01-01
weight: 11
chapter: false
pre: " <b> 5.11. </b> "
---

Clean up every short-lived resource immediately after an authorized live demo. Retained evidence should not require active compute.

## 1. Remove API Gateway and Lambda

```bash
python lambda/deploy_api_gateway.py \
  --base-name agent-risk-score \
  --endpoint-name agent-risk-local-xgboost-endpoint \
  --region ap-southeast-1 \
  --cleanup
```

## 2. Remove Endpoint Resources

```bash
python inference/deploy_sagemaker_endpoint.py \
  --bucket "<ap-southeast-1-serving-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region ap-southeast-1 \
  --model-name agent-risk-local-xgboost \
  --cleanup
```

The helper requests deletion in dependency order: Endpoint → Endpoint Config → Model. Endpoint deletion is asynchronous. Wait until the Endpoint disappears, then run the same cleanup command once more if the configuration or model remained dependent during the first call.

## 3. Remove Monitoring UI Resources

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

## Final Absence Checklist

- No demo Endpoint, Endpoint Config, or SageMaker Model.
- No temporary Lambda function or API Gateway HTTP API.
- No Model Monitor schedule or temporary monitoring Endpoint.
- No CloudWatch dashboard or alarms created for the demo.
- No running Studio app.
- No active Processing, Training, HPO, or Pipeline execution.
- No credentials exposed in logs, screenshots, or trajectory data.

## External/OOD Data Handling

The local External/OOD pilot created no AWS resource, so it requires no AWS cleanup. Raw public trajectories and annotation packages remain outside the Hugo site and are not copied into this repository. The website publishes only concise aggregate metrics and a redacted false-negative summary.

## Retain as Evidence

Keep only the required S3 raw/processed data, model artifacts, held-out evaluation reports, Pipeline/HPO metadata, Data Capture records, Model Monitor baseline/reports, and relevant CloudWatch logs/metrics. Apply appropriate lifecycle and log-retention policies rather than deleting accepted submission evidence.

At the final accepted check, serving/API resources, the monitoring schedule, dashboard, alarms, temporary monitoring Endpoint, and Studio apps were absent.

## Accepted Cleanup Evidence

![SageMaker Endpoint absent after cleanup](/images/5-Workshop/current/cleanup-sagemaker-endpoint-absent.png)

*Figure 1. The demo Endpoint is absent after cleanup.*

![Lambda functions absent after cleanup](/images/5-Workshop/current/cleanup-lambda-functions-absent.png)

*Figure 2. No demo Lambda function remains.*

![API Gateway APIs absent after cleanup](/images/5-Workshop/current/cleanup-apigateway-apis-abesent.png)

*Figure 3. No demo API Gateway HTTP API remains.*

![Model Monitor schedule absent after cleanup](/images/5-Workshop/current/cleanup-model-monitor-schedule-absent.png)

*Figure 4. No Model Monitor schedule remains active.*

![SageMaker Studio zero running applications after cleanup](/images/5-Workshop/current/cleanup-studio-zero-running-apps.png)

*Figure 5. The final Studio check shows zero running applications.*
