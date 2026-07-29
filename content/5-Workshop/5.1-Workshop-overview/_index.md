---
title: "Workshop Overview"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

AI Coding Agents can read code, modify files, run commands, execute tests, and summarize results. These capabilities are useful, but a convincing final answer does not prove that the underlying behavior was safe or correct.

## Problem and Decision

The project evaluates the full trajectory: files read and modified, tools and commands used, diff size, test/lint evidence, sensitive-file access, destructive actions, and whether the final claim is supported.

A representative response is:

```json
{
  "risk_score": 0.6003,
  "quality_score": 0.3997,
  "predicted_label": "failed",
  "decision": "require_review"
}
```

The score supports a reviewer; it does not replace human judgment. Deterministic hard rules still protect sensitive files and destructive commands.

## Completed Scope

The accepted implementation covers:

1. Deterministic simulator and SWE-bench Lite pseudo-trajectory training sources, plus Mini LLM Agent demo trajectories.
2. Amazon S3 and SageMaker Processing with a shared 17-feature contract.
3. Managed SageMaker XGBoost Training and held-out evaluation.
4. SageMaker Experiments and bounded Random HPO.
5. A Pipeline gate at `risky_recall >= 0.85` and conditional Model Registry registration.
6. Historical short-lived Endpoint, Lambda, and API Gateway serving.
7. Endpoint Data Capture, Lambda EMF, Model Monitor, and CloudWatch acceptance.
8. Cleanup of paid serving and monitoring resources while retaining evidence.
9. A separate local External/OOD diagnostic over 40 pinned public trajectories using the frozen 17-feature model.

## Split-Region and Governance Boundary

The project could not remain entirely in `ap-southeast-1` because the required SageMaker Training quota was not approved there. The quota for `1 x ml.m5.large` was approved in `us-east-1`, so managed Training and its dependent governance workflow moved to that Region.

- `us-east-1`: managed Training, evaluation artifacts, Experiments/HPO, Pipeline, Model Registry, and Model Monitor acceptance.
- `ap-southeast-1`: historical Processing/Studio and short-lived Endpoint, Lambda, API Gateway, and CloudWatch serving acceptance.

Model packages `agent-risk-scorer/1` and `/2` remain `PendingManualApproval`. The Pipeline did not approve or deploy either package. The historical Endpoint used an earlier locally trained artifact and must not be presented as a Registry deployment.

## Evidence Limitation

The held-out metrics are perfect because the current dataset is mostly synthetic and intentionally separable. They validate that the managed workflow executed correctly, not production model quality or real-world generalization. Real trajectories and human labeling are required before production use.
