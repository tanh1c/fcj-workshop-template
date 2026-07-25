---
title: "Week 6: XGBoost Training and Evaluation"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

## 06/07/2026 - 12/07/2026

**Work mode:** Individual implementation combined with group-based learning and discussion.
**Program:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** No fixed mentor assigned; work was self-managed and supported by documentation, tutorials, and peer discussion.

## Objective

Train the first scoring model and evaluate it with metrics that prioritize risky-run detection and hallucinated-success detection.

## Context

After feature engineering, the project needed a supervised model that could turn tabular behavior features into a decision signal. XGBoost was selected because it works well for structured tabular data and is supported by SageMaker.

## AWS Learning Focus

This week focused on model training concepts on AWS and the practical limitation of student-account quotas. I studied the intended SageMaker Training workflow before deciding to use local XGBoost as a fallback.

- **SageMaker Training purpose:** Learned that SageMaker Training runs model training jobs on managed infrastructure and writes trained artifacts back to S3.
- **Built-in XGBoost:** Reviewed SageMaker support for XGBoost and why it fits tabular classification tasks.
- **Training input channels:** Studied how training and validation CSV files from S3 are passed into a training job.
- **Training output artifact:** Reviewed how a completed training job normally produces model artifacts stored in S3.
- **Instance type selection:** Learned that training jobs require available ML instance quotas, which can differ by account and Region.
- **AWS Service Quotas:** Checked the quota limitation and recorded that the student account could not run the planned managed training job.
- **Local fallback:** Used local XGBoost training to keep the project moving while documenting SageMaker Training as planned but unavailable.
- **Evaluation focus:** Studied why safety-oriented metrics such as risky recall and false-negative rate are more meaningful than accuracy alone for a risk-scoring project.

This week demonstrates that I studied the AWS managed training path first, then made an implementation decision based on real account constraints rather than presenting an inaccurate result.

> **Historical state of this week:** The `ap-southeast-1` Training quota was unavailable during Week 6, so local XGBoost kept the project moving. After a separate `ml.m5.large` quota was approved in `us-east-1`, managed Training later completed in Week 8. The screenshots below remain valid evidence of the Week 6 decision, not the final managed lifecycle.

## Daily Breakdown

| Date | Work performed |
|---|---|
| 06/07/2026 | Prepared processed CSV files for model training and selected XGBoost for tabular classification. |
| 07/07/2026 | Attempted to align training with SageMaker Training and checked account quota limitations. |
| 08/07/2026 | Used local XGBoost training as the fallback path when managed training quota was unavailable. |
| 09/07/2026 | Generated evaluation metrics including accuracy, macro F1, risky recall, and false-negative related metrics. |
| 10/07/2026 | Packaged model outputs and verified the model artifact folder. |
| 11/07/2026 - 12/07/2026 | Captured evaluation, model artifact, and quota screenshots for honest MVP documentation. |


## Technical Activities

- Prepared the training data from the processed CSV files and selected target labels for classification.
- Aligned the planned workflow with SageMaker Training, while documenting that managed training quota was blocked in the student AWS account.
- Used local XGBoost training as the practical fallback so the project could still produce a model artifact and continue to endpoint deployment.
- Evaluated accuracy, macro F1, risky recall, risky false-negative rate, and hallucinated-success recall, with emphasis on false negatives because missing risky behavior is more serious than flagging a run for review.

## Deliverables

- **XGBoost model trained locally.**
- **Evaluation report produced.**
- **Week 6 quota limitation documented; managed retry deferred to a later week.**
- **Model artifact prepared for packaging.**

## Challenge and Solution

**Challenge:** The AWS account quota prevented the intended SageMaker Training job, so the project had to avoid pretending that managed training was completed.

**Solution:** The report preserves local training as the honest Week 6 fallback. Managed SageMaker Training is recorded later, when the separate `us-east-1` quota became available and the job actually completed.

## Project Relevance

This week contributed to the final MVP by strengthening the path from **AI coding-agent behavior evidence** to an **AWS-based risk scoring workflow**. The work helped ensure that the final workshop is not only a conceptual explanation, but also follows the actual implementation sequence used in the project.

## Evidence Screenshots

![XGBoost evaluation report](/images/worklog/week06-xgboost-evaluation.png)

![Local model artifact folder](/images/worklog/week06-model-artifact-local.png)

![SageMaker Training quota evidence](/images/worklog/week06-sagemaker-training-quota.png)

The screenshots show the local XGBoost evaluation result, the generated model artifact, and the SageMaker Training quota evidence that explains why local training was used as the practical fallback.

## Evidence and References Studied

- [XGBoost algorithm with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)
- [SageMaker training jobs](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html)

---

[Previous](/1-worklog/1.5-week5/) | [Back to Worklog](/1-worklog/) | [Next](/1-worklog/1.7-week7/)
