---
title: "Proposal"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 2. </b> "
---

## Project Overview

The original proposal was to build an **end-to-end risk scoring and quality evaluation system for AI coding agents on AWS SageMaker**. The completed project now demonstrates that workflow with managed training, evaluation, governance, serving evidence, and monitoring acceptance while keeping human review and deterministic safety rules in control of high-risk decisions.

A coding agent can read source files, modify code, run commands and tests, and summarize its work. Those actions create useful operational evidence, but also expose risk when the agent touches sensitive files, attempts destructive commands, skips verification, or claims success without supporting evidence. This project records the trajectory, converts it into a shared 17-feature contract, and combines an XGBoost risk score with hard safety signals.

## Problem Statement

A final answer such as “tests passed” is not trustworthy by itself if the trajectory contains no test command, unrelated edits, or unsafe actions. The project therefore addresses this question:

> How can the quality and risk of an AI coding-agent run be assessed from its behavior evidence through a governed AWS ML workflow?

The assessment focuses on whether the agent:

- Read and modified files relevant to the task.
- Verified changes with tests or linting.
- Used a valid tool sequence and supported its final claims.
- Touched sensitive files, used network commands, or attempted destructive commands.

## Objectives and Implemented Scope

The completed implementation includes:

- An agent-neutral trajectory schema using JSON and JSONL records.
- Controlled data generation from a simulator and Mini LLM Coding Agent, plus a SWE-bench Lite pseudo-trajectory source.
- A 17-feature contract shared by Processing, Training, inference, and Model Monitor.
- Amazon S3 storage for raw data, processed splits, reports, captured requests, and model artifacts.
- SageMaker Processing and Pipeline preprocessing for train, validation, and test datasets.
- Managed SageMaker XGBoost Training, held-out evaluation, Experiments, and automatic model tuning in `us-east-1`.
- A SageMaker Pipeline with a `risky_recall >= 0.85` registration gate.
- SageMaker Model Registry versions retained as `PendingManualApproval`.
- Historical real-time serving acceptance through a short-lived SageMaker Endpoint, Lambda, and API Gateway in `ap-southeast-1`.
- Endpoint Data Capture, CloudWatch operational and safety metrics, alarms, dashboard evidence, and Model Monitor acceptance.
- Cleanup procedures for short-lived paid resources.
- A local External/OOD diagnostic over 40 public trajectories sampled `20 + 20` from pinned revisions with seed `42`, using independent AI-assisted A/B annotation plus adjudication and the frozen 17-feature model without retraining, threshold tuning, or AWS calls.

### Non-Goals

- Claiming production model quality from the current mostly synthetic dataset.
- Automatically approving or deploying a model after it passes one metric gate.
- Replacing human review with an ML score.
- Keeping a real-time Endpoint or other paid demo resources running continuously.
- Delivering a complete enterprise CI/CD or multi-account production platform.

## Implemented AWS Architecture

![Completed split-Region AWS architecture for AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-architecture.webp)

*Figure 1. Completed split-Region architecture. Managed ML and governance evidence is retained in `us-east-1`; the short-lived serving demonstration and its operational evidence were accepted in `ap-southeast-1`.*

The project could not remain entirely in `ap-southeast-1` because the required SageMaker Training quota was not approved there. The quota for `1 x ml.m5.large` was approved in `us-east-1`, so managed Training and its dependent evaluation, HPO, Pipeline, Model Registry, and Model Monitor workflow ran in `us-east-1`. Historical Processing, Studio, and short-lived serving evidence remained in `ap-southeast-1`.

| Region | Accepted workload |
|---|---|
| `us-east-1` | Managed XGBoost Training, held-out evaluation artifacts, Experiments/HPO, SageMaker Pipeline, Model Registry, and Model Monitor acceptance. |
| `ap-southeast-1` | Historical SageMaker Studio and Processing work, plus the short-lived Endpoint, Lambda, API Gateway, Data Capture, and CloudWatch serving evidence. |

The Pipeline deliberately stops at governed registration. Model packages `agent-risk-scorer/1` and `agent-risk-scorer/2` completed with `PendingManualApproval`; neither package was approved or deployed to an Endpoint.

The historical Endpoint/API acceptance used an earlier locally trained XGBoost artifact. It is shown as a separate serving track so that the managed Registry artifacts are not misrepresented as deployed models. Endpoint, API, Lambda, monitoring schedule, dashboard, alarms, and Studio app were cleaned up after evidence collection; S3 artifacts, reports, captured requests, logs, and metrics remain as durable evidence.

## Data and ML Lifecycle

![Completed governed data and ML lifecycle for AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-ml-flow.webp)

*Figure 2. Implemented data and ML lifecycle with managed Training, held-out evaluation, HPO, a risky-recall gate, and manual approval before any separate release operation.*

The governed lifecycle is:

1. The simulator, Mini LLM Coding Agent, and SWE-bench Lite adapter produce trajectory JSON/JSONL records.
2. Raw records are stored in Amazon S3.
3. SageMaker Processing converts trajectories into the shared 17-feature contract and creates train, validation, and test CSV files.
4. SageMaker XGBoost `1.7-1` runs managed Training on `1 x ml.m5.large` in `us-east-1`.
5. Held-out evaluation writes accuracy, macro F1, risky recall, risky false-negative rate, and hallucinated-success recall artifacts.
6. SageMaker Experiments and a three-child-job HPO run retain tuning evidence and selected hyperparameters.
7. The Pipeline evaluates `risky_recall >= 0.85`.
8. A passing execution registers the model as `PendingManualApproval`; a failing execution stops without registration.
9. Manual approval and deployment remain separate release decisions. The Pipeline contains no Endpoint deployment step.

## Shared Feature Contract

The model-ready record uses these 17 fields:

```json
{
  "num_files_read": 2,
  "num_files_modified": 1,
  "num_tools_called": 4,
  "num_commands_run": 1,
  "diff_total_lines": 7,
  "task_file_relevance_score": 0.95,
  "latency_total_ms": 1250,
  "tests_passed": 1,
  "lint_passed": 1,
  "touched_sensitive_files": 0,
  "destructive_command_detected": 0,
  "used_network_command": 0,
  "summary_claim_supported": 1,
  "tool_sequence_valid": 1,
  "source_simulator": 0,
  "source_mini_llm_agent": 1,
  "source_swe_bench_lite": 0
}
```

A representative `POST /score-agent-run` response is:

```json
{
  "risk_score": 0.6003,
  "quality_score": 0.3997,
  "predicted_label": "failed",
  "decision": "require_review"
}
```

The model score assists review; it is not the only safety control. Destructive commands and sensitive-file behavior remain subject to deterministic rules even when model confidence is low.

## AWS Services Used

| AWS Service | Implemented usage | Why selected |
|---|---|---|
| Amazon S3 | Stores raw trajectories, processed splits, evaluation reports, captured requests, monitoring reports, and model artifacts. | Durable object storage provides one low-cost interface for data and artifacts across the ML lifecycle. |
| Amazon SageMaker Studio | Supported historical development and Console evidence. | Provides an AWS-integrated workspace for development and inspection of SageMaker resources. |
| SageMaker Processing | Performs feature engineering, dataset splitting, evaluation processing, and monitoring baseline work. | Runs repeatable managed data jobs with S3 inputs and outputs without maintaining a processing server. |
| SageMaker Training | Runs managed XGBoost Training in `us-east-1`. | Provides an isolated managed XGBoost job with recorded configuration, metrics, artifacts, and billable duration. |
| SageMaker Experiments and HPO | Tracks the experiment, tuning job, child jobs, metrics, and selected hyperparameters. | Preserves comparable trial metadata while bounding automatic tuning to three serial child jobs. |
| SageMaker Pipelines | Orchestrates preprocess, train, evaluate, metric check, and conditional registration. | Makes the ML workflow reproducible and enforces the risky-recall registration gate. |
| SageMaker Model Registry | Retains completed model packages under `agent-risk-scorer` with manual approval required. | Adds model versioning and a governance boundary between registration, approval, and deployment. |
| SageMaker Endpoint and Runtime | Provided short-lived historical real-time inference using the earlier local artifact. | Supports synchronous scoring for the live API demonstration; it remained short-lived to control cost. |
| SageMaker Model Monitor | Produced an accepted baseline, data-quality execution, violations, and CloudWatch data metrics. | Provides managed baseline comparison and drift evidence integrated with SageMaker and CloudWatch. |
| AWS Lambda | Converts trajectories into the feature payload, invokes SageMaker Runtime, and emits Embedded Metric Format safety metrics. | Serverless execution fits the request-driven adapter and avoids maintaining an API server. |
| Amazon API Gateway | Exposed the historical HTTP API route `POST /score-agent-run`. | Provides a managed HTTP entry point for Lambda without a separate web server. |
| Amazon CloudWatch | Retained native service metrics, `AgentRiskScorer` metrics, logs, dashboard evidence, and seven actions-disabled alarms. | Centralizes AWS-native logs, metrics, dashboards, and alarms for operating the scoring path. |
| AWS IAM | Separates CLI, SageMaker execution, and Lambda execution permissions using least-privilege roles. | Service-specific roles reduce credential exposure and limit each component to its required actions. |

## Accepted Implementation Evidence

| Area | Accepted result |
|---|---|
| Managed Training | Job `agent-risk-xgboost-1784625353` completed on `1 x ml.m5.large` with SageMaker XGBoost `1.7-1`; training and billable time were 140 seconds. |
| Held-out evaluation | 183 test rows; accuracy `1.00`, macro F1 `1.00`, risky recall `1.00`, risky false-negative rate `0.00`, and hallucinated-success recall `1.00`. |
| HPO | Random tuning job `agent-risk-hpo-1784643415` completed three child jobs and retained the selected configuration in SageMaker Experiments. |
| Pipeline and Registry | Conditional registration completed; package versions `/1` and `/2` remain `PendingManualApproval`. |
| Serving API | The short-lived Endpoint, Lambda, and API Gateway returned risk-aware decisions through `POST /score-agent-run`. |
| Data Capture | Captured JSON input and output at 100% sampling into S3. |
| Model Monitor | Baseline covered 854 rows and 17 features; the accepted run was `CompletedWithViolations` and reported real drift in `diff_total_lines` and `latency_total_ms`. |
| CloudWatch | Published Lambda EMF metrics and 101 Model Monitor data metrics; dashboard and seven alarms were accepted before cleanup. |
| External/OOD diagnostic | 40 public samples; A/B full-axis agreement `3/40 = 7.5%`; 37 adjudicated; final labels `failed=28`, `safe=10`, `risky=2`; accuracy `0.0500`, macro F1 `0.1212`, risky recall `0.5000`, and risky FNR `0.5000`. The frozen model was evaluated locally without retraining or AWS calls. |

The perfect held-out scores reflect mostly synthetic, intentionally separable labels. They demonstrate that the managed ML and MLOps workflow executes correctly; they do not establish production accuracy or generalization to real repositories.

## Implementation Timeline and Status

| Phase | Completed work |
|---|---|
| Foundation | Defined the risk-scoring problem, trajectory schema, safety rules, IAM boundaries, and S3 structure. |
| Data | Built simulator, Mini LLM Agent, SWE-bench Lite adapter, feature extraction, and Processing outputs. |
| Managed ML | Completed Training, held-out evaluation, Experiments, HPO, Pipeline, and conditional Registry integration in `us-east-1`. |
| Serving | Accepted short-lived Endpoint, Lambda, and API Gateway behavior in `ap-southeast-1` using the historical local artifact. |
| Monitoring | Accepted Data Capture, EMF metrics, CloudWatch dashboard/alarms, and Model Monitor evidence. |
| Delivery | Collected evidence, cleaned up paid resources, and completed the report, demo runbook, and bilingual workshop. |

## Budget

The project was funded with promotional AWS credit. Both supplied Cost Explorer views display `$0.00`; this is the displayed net result, not evidence that the AWS resources had no economic cost. The captured Billing views do not provide a defensible gross usage total or per-service breakdown, so neither is estimated. The exact credit balance, currency, status, and expiration are also omitted because they could not be transcribed reliably from the supplied Credits screenshot. The Billing screenshots are retained as private submission evidence rather than published on this website.

| Cost evidence or control | Recorded scope |
|---|---|
| Managed Training | `1 x ml.m5.large` for 140 billable seconds. |
| HPO | Bounded to three serial child jobs. |
| Historical serving | Short-lived `ml.t2.medium` real-time Endpoint. |
| Processing and monitoring | Processing and Model Monitor jobs ran only when required for acceptance evidence. |
| Cleanup | Endpoint, Lambda, API Gateway, Model Monitor schedule, dashboard, alarms, and Studio apps were removed after verification. |
| Retained resources | Only low-cost durable S3 artifacts, reports, captures, logs, and metrics were retained. |

## Cost and Security Controls

- Paid Processing, Training, HPO, Pipeline, Endpoint, and Model Monitor work was run only when needed for acceptance evidence.
- The Endpoint and serving stack were short-lived; dashboard, alarms, monitoring schedule, and Studio app were also removed after verification.
- Low-cost S3 artifacts, reports, captures, logs, and metrics were retained as evidence.
- The Mini Agent has no unrestricted generic shell tool.
- CLI, SageMaker, and Lambda responsibilities use separate IAM identities or roles.
- Registry approval remains manual, and a metric gate never authorizes automatic production deployment.

## Risks, Limitations, and Mitigations

| Risk or limitation | Mitigation |
|---|---|
| Mostly synthetic and deliberately separable labels can inflate metrics. | Treat results as workflow validation; the observed drop from synthetic macro F1 `1.00` to external `0.1212` confirms a substantial distribution shift. |
| External labels remain uncertain. | Treat the 40-sample pilot as a diagnostic because labels are multi-agent/AI-assisted and full-axis A/B agreement was only `7.5%`; collect independent human labels before production claims. |
| Only two External/OOD samples are risky. | Report the wide risky-recall Wilson 95% interval `[0.0945, 0.9055]`; do not tune the threshold from this pilot. |
| Domain or behavior drift can reduce model reliability. | Review parser/default-value behavior, collect a larger representative dataset, and perform governed retraining and evaluation only after human labeling. |
| ML can miss dangerous actions. | Keep hard rules for destructive commands and sensitive files, and require human review for risky decisions. |
| Cross-Region resources can imply a deployment path that did not occur. | Document managed governance and historical serving as separate tracks; perform deployment only as an explicit release operation. |
| Real-time and monitoring resources create ongoing cost. | Use short acceptance windows, confirmation gates, and immediate cleanup. |
| Model/runtime dependency versions can diverge. | Pin compatible evaluation and inference environments for future managed releases. |

## Deliverables

- Bilingual Proposal, final report, architecture description, demo script, and evidence inventory.
- Agent-neutral trajectory schema and three controlled data sources.
- Shared 17-feature processing, training, inference, and monitoring contract.
- Managed Training, evaluation, Experiments/HPO, Pipeline, and Registry evidence.
- Historical serverless scoring API and real-time inference evidence.
- Data Capture, Model Monitor, CloudWatch dashboard, metric, and alarm evidence.
- Cleanup procedures and retained durable artifacts.
- External/OOD machine-readable evidence with annotation coverage, per-source metrics, a rule-only baseline, and one redacted risky false negative; raw public trajectories and annotation packages remain outside the website.

## Future Work

Future work should begin with a larger representative trajectory dataset, independent human labeling, and review of parser/default-value behavior. Only then should the project perform governed retraining and evaluation, calibration or cost-sensitive learning, compatible pinned runtime images, and a reviewed release process for deploying an approved Registry model. Production adoption would also require CI/CD controls, security review, access auditing, and cost governance beyond this internship scope.

## References Studied

- [Amazon SageMaker Processing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)
- [XGBoost algorithm with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)
- [SageMaker real-time inference](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints.html)
- [Invoke SageMaker endpoints](https://docs.aws.amazon.com/sagemaker/latest/dg/realtime-endpoints-test-endpoints.html)
- [Using Lambda with API Gateway](https://docs.aws.amazon.com/lambda/latest/dg/services-apigateway.html)
- [HTTP APIs in API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/http-api.html)
- [IAM roles for AWS services](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
