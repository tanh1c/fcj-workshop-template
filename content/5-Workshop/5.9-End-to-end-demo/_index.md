---
title: "End-to-End Validation and Evidence"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

Validate the workflow evidence-first. Free local checks and retained AWS records are sufficient; a live serving stack is optional and requires explicit authorization.

## Free Local Validation

```bash
PYTHONPATH=demo_repo pytest demo_repo/tests -v
pytest agent data_generation preprocessing training inference pipelines lambda monitoring -q

python preprocessing/processing_script.py \
  --input data_generation/combined_trajectories.jsonl \
  --output-dir data/processed \
  --seed 42
```

Confirm the trajectory schema, 17-feature order, labels, train/validation/test splits, and local test results.

## External/OOD Evidence Review

Review the retained local evidence without regenerating annotations or fetching public trajectories during the demo:

1. Open `report/external_eval/external_pilot_report.json` at `coverage`, `overall`, and `by_source`.
2. Open `report/external_eval/false_negatives.jsonl` and show only the redacted record.
3. Explain the drop from synthetic macro F1 `1.00` to external macro F1 `0.1212`.
4. State that no retraining, threshold tuning, SageMaker call, or AWS Pipeline execution occurred.

This evidence is enough to demonstrate the generalization gap. The raw public trajectories and annotation packages are not part of the website or video flow.

## Accepted AWS Evidence Checklist

- Processing Job completed and retained train/validation/test CSVs.
- Managed XGBoost Training Job completed on `1 x ml.m5.large`.
- Held-out evaluation report covers 183 rows and includes safety metrics.
- Random HPO completed three child jobs and retained the selected metadata.
- Pipeline execution `z9y3p0bqaske` succeeded through conditional registration.
- Registry versions `/1` and `/2` remain `PendingManualApproval` and undeployed.
- Historical Endpoint and `POST /score-agent-run` returned HTTP `200` before cleanup.
- S3 retains JSON Data Capture evidence.
- Model Monitor recorded `CompletedWithViolations` and retained reports.
- CloudWatch retains metrics/logs; the accepted dashboard and seven alarms were cleaned up.

## Optional Live Serving Sequence

After the confirmation gate:

1. Deploy one short-lived `ml.t2.medium` Endpoint with Data Capture.
2. Invoke it directly once.
3. Deploy Lambda and the HTTP API.
4. Use only the newly printed URL for one or two Mini Agent requests.
5. Inspect `runs/run_login_api.json` and its `score_response`.
6. Clean up API/Lambda first, then Endpoint resources.

Do not rerun Processing, Training, HPO, Pipeline, or Model Monitor for the live sequence. If the Endpoint does not reach `InService`, stop, clean up partial resources, and present retained evidence. Do not retry blindly or switch to a larger instance.

## Interpretation

A successful request proves integration of the historical serving path. It does not prove that the managed Registry packages were deployed, and perfect synthetic evaluation metrics do not prove production generalization.
