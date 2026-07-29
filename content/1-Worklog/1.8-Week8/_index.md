---
title: "Week 8: Managed ML Governance and Accepted Evidence"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

## 20/07/2026 - 26/07/2026

**Work mode:** Individual implementation combined with group-based learning and discussion.  
**Program:** Workforce Bootcamp - First Cloud AI Journey.  
**Mentor:** No fixed mentor assigned; work was self-managed and supported by documentation, tutorials, and peer discussion.

> **Status as of 25/07/2026:** This week is still in progress. The completed entries below cover work from 20–25 July; the item for 26 July remains planned and is not reported as completed.

## Objective

Complete and reconcile the managed ML, governance, historical serving, and observability evidence without implying that registered model packages were deployed.

## Context

This week closed the gap between the earlier local fallback and the completed managed workflow. Managed Training, HPO, Pipeline, Registry, and Model Monitor acceptance ran in `us-east-1`. Historical short-lived serving, API, Data Capture, and Lambda/CloudWatch evidence remained a separate `ap-southeast-1` track using the earlier local artifact.

## Daily Breakdown

| Date | Status and work |
|---|---|
| 20/07/2026 | Completed managed XGBoost `1.7-1` Training in `us-east-1` on `1 × ml.m5.large` and evaluated 183 held-out rows. |
| 21/07/2026 | Completed three-job serial Random HPO, selected the best trial, and accepted the governed Pipeline execution and Registry version `/2`. |
| 21/07/2026 | Accepted separate `ap-southeast-1` historical serving, HTTP `200` scoring, 100% JSON Data Capture, `AgentRiskScorer` EMF, Model Monitor, and CloudWatch evidence; then cleaned temporary resources. |
| 22/07/2026 | Reconciled managed Registry versions `/1` and `/2` as `Completed` and `PendingManualApproval`; confirmed neither was approved or deployed. |
| 23/07/2026 | Cross-checked reports, Workshop content, metrics, safety limitations, and cleanup state. |
| 24/07/2026 | Completed final-report reconciliation and the bounded local External/OOD pilot with multi-agent A/B annotation, adjudication, frozen-model evaluation, and report/demo updates. No paid AWS job or serving resource was run. |
| 25/07/2026 | Attended the AWS Vietnam Community Meetup in person at AWS Hà Nội and recorded lessons about AI-assisted engineering, open-source agent runtimes, human review, and AI-native infrastructure. |
| 26/07/2026 | **Planned:** Gather reviewer feedback and correct documentation only; no AWS rerun is required. |

## Managed Training, Evaluation, and HPO

- Training job `agent-risk-xgboost-1784625353`: `Completed`, 140 billable seconds, XGBoost `1.7-1`, `1 × ml.m5.large`.
- Held-out evaluation: 183 rows, macro F1 `1.00`, risky recall `1.00`, risky false-negative rate `0.00`.
- HPO `agent-risk-hpo-1784643415`: Random strategy, three serial completed child jobs, selected `agent-risk-hpo-1784643415-001-59146c4e`.

The intentionally separable synthetic labels explain the perfect metrics. These results validate workflow execution, not production generalization.

![Managed Training, held-out evaluation, and HPO evidence summary](/images/worklog/week08-managed-training-hpo-evidence.svg)

## Pipeline and Model Registry Governance

Pipeline execution `z9y3p0bqaske` succeeded through:

```text
Preprocess → Train → Evaluate → CheckRiskyRecall
```

The gate required `risky_recall >= 0.85`; observed risky recall was `1.00`, so conditional registration was permitted. Registry versions `/1` and `/2` are `Completed` and `PendingManualApproval`. Neither package was approved or deployed, and the Pipeline has no Endpoint deployment step.

![Pipeline safety gate and Model Registry evidence summary](/images/worklog/week08-pipeline-registry-evidence.svg)

## Historical Serving and Request Evidence

The temporary `ap-southeast-1` Endpoint used the earlier local artifact, not either managed Registry package. `POST /score-agent-run` returned HTTP `200` with a representative `require_review` decision. Endpoint Data Capture retained 100% of JSON input/output, while Lambda emitted Embedded Metric Format records under `AgentRiskScorer`.

![Historical serving, API, Data Capture, and EMF evidence summary](/images/worklog/week08-serving-observability-evidence.svg)

## Model Monitor, CloudWatch, and Cleanup

The accepted Model Monitor baseline contained 854 rows and 17 features. The one-time execution finished as `CompletedWithViolations`, with drift in `diff_total_lines` and `latency_total_ms`. Two additional type findings were retained as an honest limitation of boundary-valued demo traffic rather than hidden.

CloudWatch exposed 101 Model Monitor data metrics. The accepted dashboard and seven actions-disabled alarms used missing-data-safe behavior and were deleted after verification. Temporary Endpoint, API, monitoring schedule, dashboard, and alarm resources are absent; reports, captures, logs, and metrics remain as evidence.

![Model Monitor, CloudWatch, and cleanup evidence summary](/images/worklog/week08-monitoring-cleanup-evidence.svg)

## External/OOD Diagnostic — 24/07/2026

The local pilot sampled `20 + 20` public trajectories from pinned revisions with seed `42`. Two independent AI-assisted annotators achieved full-axis A/B agreement on `3/40 = 7.5%`; 37 samples were adjudicated, with none excluded or pending. The frozen model reached external macro F1 `0.1212`, risky recall `0.5000`, and risky false-negative rate `0.5000`.

The pilot did not retrain the model, tune its threshold, call SageMaker, or run external data through the AWS Pipeline. It exposed a generalization gap without changing the accepted AWS lifecycle or cleanup state.

## Deliverables Through 25/07/2026

- **Managed Training and held-out evaluation accepted.**
- **Bounded HPO and selected best trial recorded.**
- **Governed Pipeline and two Registry versions reconciled.**
- **Historical serving/API and Data Capture evidence retained separately.**
- **Model Monitor, CloudWatch, and cleanup evidence reconciled.**
- **Final report reconciled with the local External/OOD pilot and demo narrative.**
- **Multi-agent annotation/adjudication and frozen-model evaluation recorded without an AWS rerun.**
- **Event 4 attended in person with reflection on AI engineering, agent safety, and AI-native infrastructure.**

## Decision Boundary

The model score is advisory. Human review and hard safety rules remain authoritative, especially for destructive commands, sensitive paths, unsupported success claims, and uncertain evidence.

## Evidence Sources

The four cards above are static summaries derived from the accepted local project records in `report/demo_evidence.md` and `report/best_model_metrics.json`. They are not AWS Console screenshots and contain no credentials, full ARNs, account-specific bucket names, or inactive API URLs.

---

[Previous](/1-worklog/1.7-week7/) | [Back to Worklog](/1-worklog/) | [Next](/1-worklog/1.9-week9/)
