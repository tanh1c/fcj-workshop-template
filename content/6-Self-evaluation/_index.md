---
title: "Self-Assessment"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

## Self-Assessment

During my internship at **Amazon Web Services Viet Nam Company Limited** in the **Workforce Bootcamp - First Cloud AI Journey** program from **01/06/2026 to 23/08/2026**, I had the opportunity to study AWS services, join technical events, and build a self-developed project named **End-to-End Risk Scoring and Quality Evaluation System for AI Coding Agents on AWS SageMaker**.

The internship helped me connect university knowledge with a practical cloud and AI workflow. SageMaker Training quota was unavailable in `ap-southeast-1`, so an earlier local artifact supported historical serving there. After quota for `1 x ml.m5.large` was approved in `us-east-1`, I completed managed Training, held-out Evaluation, Experiments/HPO, Pipeline, Model Registry, and Model Monitor acceptance while retaining Processing, Endpoint, Lambda, API Gateway, Data Capture, IAM, and CloudWatch evidence.

In terms of work ethic, I worked independently while learning from group discussions, AWS documentation, workshops, and community events. I focused on completing the governed workflow, collecting durable evidence, cleaning up paid resources, writing a bilingual workshop website, and presenting limitations transparently.

## Evaluation Criteria

| No. | Criteria | Description | Good | Fair | Average |
|---|---|---|---|---|---|
| 1 | **Professional knowledge & skills** | Understanding of AWS services, ML workflow, API integration, and applying technical knowledge to the project | ✅ | ☐ | ☐ |
| 2 | **Ability to learn** | Ability to study new AWS services, read documentation, test commands, and learn from errors | ✅ | ☐ | ☐ |
| 3 | **Proactiveness** | Taking initiative to choose a realistic project topic, define MVP scope, and collect evidence | ✅ | ☐ | ☐ |
| 4 | **Sense of responsibility** | Completing project sections, documenting limitations, and keeping cost cleanup in mind | ✅ | ☐ | ☐ |
| 5 | **Discipline** | Following the internship timeline, maintaining weekly worklog entries, and organizing deliverables | ☐ | ✅ | ☐ |
| 6 | **Progressive mindset** | Willingness to receive feedback, revise content, improve diagrams, and update the report structure | ✅ | ☐ | ☐ |
| 7 | **Communication** | Presenting project goals, AWS architecture, limitations, and results clearly in English and Vietnamese | ☐ | ✅ | ☐ |
| 8 | **Teamwork** | Participating in group-based learning, discussion, and community events while completing individual work | ☐ | ✅ | ☐ |
| 9 | **Professional conduct** | Respecting the learning environment, event participation, report requirements, and responsible AWS usage | ✅ | ☐ | ☐ |
| 10 | **Problem-solving skills** | Handling technical blockers such as quota limitations, API integration issues, and evidence collection after cleanup | ✅ | ☐ | ☐ |
| 11 | **Contribution to project/team** | Building a complete MVP workflow and preparing a reusable workshop-style report | ✅ | ☐ | ☐ |
| 12 | **Overall** | General evaluation of the internship period, learning attitude, project outcome, and documentation quality | ✅ | ☐ | ☐ |

## Strengths

- **Self-learning ability:** I was able to study AWS services through documentation, tutorials, CLI checks, screenshots, and practical implementation.
- **Technical implementation:** I completed the managed AWS ML/MLOps path from trajectories and Processing through Training, held-out Evaluation, Experiments/HPO, Pipeline, conditional Registry registration, historical serving, and monitoring acceptance.
- **Governance:** The `risky_recall >= 0.85` gate permits registration only; Registry versions `/1` and `/2` remain `PendingManualApproval`, while human review and deterministic safety rules remain authoritative.
- **Observability and cost control:** I accepted Data Capture, Model Monitor, CloudWatch metrics, a dashboard, and seven actions-disabled alarms, then verified cleanup of short-lived paid resources.
- **Honest evaluation:** I tested the frozen model locally on 40 external trajectories and reported the macro F1 drop from synthetic `1.00` to external `0.1212` instead of treating perfect held-out scores as production quality.
- **Evidence-based documentation:** I retained screenshots, source evidence, S3 artifacts, reports, API responses, monitoring records, and cleanup notes without publishing raw external trajectories.

## Needs Improvement

- **Communication confidence:** I should continue improving how I explain technical trade-offs verbally, especially when presenting AWS architecture and limitations to others.
- **Time management:** I need to plan documentation and screenshot collection earlier instead of leaving too much report polishing near the end.
- **Evaluation data:** The external pilot has only 40 samples, only two labeled risky, and AI-assisted labels with `7.5%` full-axis agreement; I need a larger representative dataset with independent human annotation.
- **Parser and runtime reliability:** I need to review missing-field/default-value behavior and pin a compatible runtime rather than relying on a LabelEncoder created with scikit-learn `0.24.1` and loaded under `1.8.0`.
- **Model evaluation:** Calibration or cost-sensitive learning should be considered only after stronger human-labeled data exists, followed by governed evaluation and a reviewed release after manual Registry approval.
- **Professional English writing:** I need to keep improving technical writing so the report is concise, accurate, and natural in both English and Vietnamese.

## Overall Reflection

This internship helped me understand that a cloud AI project is not only about training a model. A complete workflow also includes data design, storage structure, IAM permissions, processing jobs, model packaging, endpoint deployment, API integration, logs, cost control, cleanup, and documentation.

The most valuable lesson was learning that successful managed execution is not the same as model generalization. The AWS workflow, governance, serving, and monitoring evidence were completed, but the local External/OOD pilot exposed a large gap between synthetic and public trajectories. Reporting both results—and keeping human review, hard rules, manual approval, and cleanup boundaries explicit—helped me develop a more professional engineering mindset.
