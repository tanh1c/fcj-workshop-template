---
title: "Managed Training, Evaluation, and HPO"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Managed XGBoost Training, held-out evaluation, Experiments, and bounded HPO run in `us-east-1`. The commands below are reproducible shapes, not instructions to recreate accepted evidence.

## Managed XGBoost Training

This command creates a paid `ml.m5.large` Training Job. Run it only with explicit authorization.

```bash
python training/run_sagemaker_xgboost_training.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1 \
  --instance-type ml.m5.large
```

The launcher uses SageMaker XGBoost `1.7-1`, uploads the training and inference code, and writes `model.tar.gz` to S3.

## Held-Out Evaluation

Evaluate a completed managed artifact against its untouched `test.csv` and upload the report:

```bash
python training/run_managed_model_evaluation.py \
  --bucket "<us-east-1-training-bucket>" \
  --job-name "<completed-training-job-name>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1
```

The report includes accuracy, macro F1, risky recall, risky false-negative rate, per-class results, and a confusion matrix.

## Bounded Random HPO

Without `--start`, the launcher only prints the bounded request. Adding `--start` creates three serial paid child jobs.

```bash
python training/run_sagemaker_hpo.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1 \
  --instance-type ml.m5.large
```

## Accepted Evidence

- Training Job `agent-risk-xgboost-1784625353`: `Completed`, `1 x ml.m5.large`, 140 training and billable seconds.
- Held-out evaluation: 183 test rows, macro F1 `1.00`, risky recall `1.00`, risky false-negative rate `0.00`.
- HPO job `agent-risk-hpo-1784643415`: Random strategy, three completed serial child jobs, Experiment `agent-risk-scoring-experiment`.
- Selected child: `agent-risk-hpo-1784643415-001-59146c4e`.

![Completed managed XGBoost Training Job](/images/5-Workshop/current/training-job-agent-risk-xgboost-1784625353-completed.png)

*Figure 1. Managed Training Job `agent-risk-xgboost-1784625353` completed on `ml.m5.large`.*

![Held-out evaluation report retained in S3](/images/5-Workshop/current/s3-evaluation-report-agent-risk-xgboost-1784625353.png)

*Figure 2. S3 retains the held-out evaluation report for the accepted XGBoost job.*

![Completed bounded HPO job overview](/images/5-Workshop/current/hpo-agent-risk-hpo-1784643415-best-child-1.png)

*Figure 3. HPO job `agent-risk-hpo-1784643415` completed its bounded search.*

![HPO child jobs and selected result](/images/5-Workshop/current/hpo-agent-risk-hpo-1784643415-best-child-2.png)

*Figure 4. The HPO evidence shows the completed child-job set and selected result.*

![Best HPO child job details](/images/5-Workshop/current/hpo-agent-risk-hpo-1784643415-best-child-3.png)

*Figure 5. Child `agent-risk-hpo-1784643415-001-59146c4e` was retained as the best job.*

These perfect scores come from mostly synthetic, intentionally separable labels. They validate managed workflow execution, not production quality or generalization. The earlier local model remains relevant only as the artifact used by the historical serving demo.

## External/OOD Generalization Pilot

A separate local diagnostic tested whether the frozen 17-feature model generalized beyond the mostly synthetic internal dataset. It sampled 20 trajectories from each pinned public source with seed `42`:

| Public source | Pinned revision | Samples |
|---|---|---:|
| `nebius/SWE-agent-trajectories` | `68195a1450865274106246d0d0296a1d6807b88e` | 20 |
| `nebius/SWE-rebench-openhands-trajectories` | `35455389ab51bf5e2306bfd436ef72d0f98bf882` | 20 |

Two independent AI-assisted annotators labeled each trajectory, then an adjudicator resolved disagreements.

| Annotation coverage | Result |
|---|---:|
| Selected | `40` |
| Full-axis A/B agreement | `3/40 = 7.5%` |
| Accepted directly | `3` |
| Adjudicated | `37` |
| Excluded | `0` |
| Pending | `0` |
| Final labels | `failed=28`, `safe=10`, `risky=2` |

{{% notice warning %}}
These are multi-agent/AI-assisted labels, not independent human ground truth.
{{% /notice %}}

### Frozen-model results

| Scope | Accuracy | Macro F1 | Risky precision | Risky recall | Risky F1 | Risky FNR |
|---|---:|---:|---:|---:|---:|---:|
| Overall, 40 samples | `0.0500` | `0.1212` | `1.0000` | `0.5000` | `0.6667` | `0.5000` |
| SWE-agent, 20 samples | `0.1000` | `0.1889` | `1.0000` | `1.0000` | `1.0000` | `0.0000` |
| OpenHands, 20 samples | `0.0000` | `0.0000` | `0.0000` | `0.0000` | `0.0000` | `1.0000` |

Only two samples carried the final `risky` label, so risky recall has a wide Wilson 95% interval of `[0.0945, 0.9055]`. The result is diagnostic, not a stable production estimate.

![External OOD pilot aggregate report](/images/5-Workshop/current/local-external-ood-report-summary.png)

*Figure 6. The local report records 40 samples, macro F1 `0.1212`, risky recall `0.50`, and the source-level split.*

### Baselines and failure evidence

| Diagnostic | Samples | Accuracy | Macro F1 | Risky recall | Risky FNR |
|---|---:|---:|---:|---:|---:|
| Rule-only baseline | 40 | `0.0250` | `0.1111` | `0.5000` | `0.5000` |
| Source-only internal diagnostic (`diagnostic_only`) | 183 | `0.2077` | `0.1105` | — | — |

The source-only result is explicitly `diagnostic_only`; it checks whether legacy source indicators explain internal labels and is not an external baseline. The redacted false-negative evidence contains one OpenHands sample with true label `risky`, predicted label `require_review`, and risky probability about `0.00156`. The retained record lists high-level risk flags and parser warnings, but the website does not publish the raw public trajectory or annotation package.

![Redacted risky false-negative evidence](/images/5-Workshop/current/local-external-ood-risky-false-negative-redacted.png)

*Figure 7. The published false-negative record is redacted and contains only diagnostic fields needed to explain the miss.*

Additional limitations are material:

- All three legacy source flags are zero for unseen external sources.
- The parser uses observable evidence and conservatively preserves unknown evidence, so missing fields and defaults require review.
- The trusted LabelEncoder artifact was created with scikit-learn `0.24.1` and loaded under `1.8.0`, producing a compatibility warning.

{{% notice warning %}}
This pilot ran locally against the frozen managed artifact. It did not retrain the model, tune the threshold, call SageMaker, run external data through the AWS Pipeline, modify Registry packages, update the historical Endpoint, or replace the Model Monitor baseline.
{{% /notice %}}

The drop from synthetic macro F1 `1.00` to external macro F1 `0.1212` exposes substantial distribution shift. Before any production claim, the project needs a larger representative dataset, independent human labeling, review of parser/default-value behavior, and a new governed evaluation cycle.
