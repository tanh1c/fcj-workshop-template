---
title: "Managed Training, Evaluation và HPO"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 5.6. </b> "
---

Managed XGBoost Training, held-out evaluation, Experiments và bounded HPO chạy tại `us-east-1`. Các command dưới đây là reproducible shape, không phải yêu cầu tái tạo accepted evidence.

## Managed XGBoost Training

Command này tạo Training Job `ml.m5.large` có phí. Chỉ chạy khi được cho phép rõ ràng.

```bash
python training/run_sagemaker_xgboost_training.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1 \
  --instance-type ml.m5.large
```

Launcher dùng SageMaker XGBoost `1.7-1`, upload training/inference code và ghi `model.tar.gz` lên S3.

## Held-out Evaluation

Đánh giá managed artifact đã hoàn tất trên `test.csv` chưa được dùng và upload report:

```bash
python training/run_managed_model_evaluation.py \
  --bucket "<us-east-1-training-bucket>" \
  --job-name "<completed-training-job-name>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1
```

Report gồm accuracy, macro F1, risky recall, risky false-negative rate, per-class results và confusion matrix.

## Bounded Random HPO

Không có `--start`, launcher chỉ in bounded request. Thêm `--start` sẽ tạo ba paid child jobs chạy tuần tự.

```bash
python training/run_sagemaker_hpo.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --processed-s3-uri "<us-east-1-processed-prefix>" \
  --region us-east-1 \
  --instance-type ml.m5.large
```

## Evidence đã nghiệm thu

- Training Job `agent-risk-xgboost-1784625353`: `Completed`, `1 x ml.m5.large`, 140 training và billable seconds.
- Held-out evaluation: 183 test rows, macro F1 `1.00`, risky recall `1.00`, risky false-negative rate `0.00`.
- HPO job `agent-risk-hpo-1784643415`: Random strategy, ba child jobs chạy tuần tự hoàn tất, Experiment `agent-risk-scoring-experiment`.
- Selected child: `agent-risk-hpo-1784643415-001-59146c4e`.

Perfect scores này đến từ labels chủ yếu synthetic và được tạo để dễ phân tách. Chúng xác minh managed workflow execution, không chứng minh production quality hoặc generalization. Model local trước đó chỉ còn liên quan như artifact dùng trong historical serving demo.
