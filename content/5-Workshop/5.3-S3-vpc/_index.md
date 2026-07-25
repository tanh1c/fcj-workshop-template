---
title: "Split-Region Architecture"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

The completed project separates managed ML governance from historical serving so the evidence does not imply an unperformed deployment.

![Completed split-Region AWS architecture for AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-architecture.webp)

*Figure 1. Managed ML and governance run in `us-east-1`; historical short-lived serving and its API evidence run in `ap-southeast-1`.*

The split was required because the SageMaker Training quota needed by the project was not approved in `ap-southeast-1`. Approval for `1 x ml.m5.large` in `us-east-1` allowed managed Training and the dependent evaluation, HPO, Pipeline, Registry, and Model Monitor workflow to run there while earlier Singapore evidence remained separate.

## Region Responsibilities

| Region | Accepted workload |
|---|---|
| `us-east-1` | Managed Training, held-out evaluation artifacts, Experiments/HPO, Pipeline, Model Registry, and Model Monitor acceptance. |
| `ap-southeast-1` | Historical Processing/Studio, short-lived Endpoint, Lambda, API Gateway, Data Capture, and CloudWatch serving evidence. |

## Critical Evidence Boundary

The Pipeline registers a passing model as `PendingManualApproval`; it neither approves nor deploys it. Registry versions `agent-risk-scorer/1` and `/2` were not used by the historical Endpoint. That Endpoint used the earlier local XGBoost artifact and was deleted after acceptance.

Continue with:

- [Data and managed ML flow](/5-workshop/5.3-s3-vpc/5.3.1-create-gwe/)
- [Governance, inference, and monitoring flow](/5-workshop/5.3-s3-vpc/5.3.2-test-gwe/)
