---
title: "Pipeline và Model Registry governance"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 5.7. </b> "
---

SageMaker Pipeline tự động hóa tạo model và conditional registration tại `us-east-1`. Pipeline cố ý không có Endpoint deployment step.

## Kiểm tra Pipeline local

Command mặc định compile và in Pipeline definition mà không upsert hoặc start:

```bash
python pipelines/sagemaker_pipeline.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region us-east-1
```

Kiểm tra parameters và graph:

```text
Preprocess
  -> Train
  -> Evaluate
  -> CheckRiskyRecall
       -> RegisterModel khi risky_recall >= 0.85
       -> Dừng không registration nếu không đạt
```

Pipeline chỉ upload năm code assets khi publish rõ ràng: `processing_script.py`, `train_xgboost.py`, `evaluate_model.py`, `inference.py` và `decision_policy.py`.

## AWS mutation rõ ràng

`--upsert` tạo hoặc cập nhật Pipeline; `--start` tạo execution có phí và bị từ chối nếu thiếu `--upsert`. Không chạy command này chỉ để lấy evidence.

```bash
python pipelines/sagemaker_pipeline.py \
  --bucket "<us-east-1-training-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region us-east-1 \
  --upsert \
  --start
```

## Governance evidence đã nghiệm thu

Execution `z9y3p0bqaske` thành công qua Processing, Training, held-out Evaluation, `CheckRiskyRecall` và `RegisterModel`. Observed risky recall là `1.00` với threshold `0.85`.

![Execution graph SageMaker Pipeline đã thành công](/images/5-Workshop/current/pipeline-execution-z9y3p0bqaske-succeeded-graph.png)

*Hình 1. Execution `z9y3p0bqaske` thành công qua graph conditional registration; graph không có deployment step.*

Model Package Group `agent-risk-scorer` có versions `/1` và `/2`, đều `Completed` và `PendingManualApproval`. Vượt gate chỉ cho phép registration. Không version nào được approve hoặc deploy, và workshop không cung cấp approval command. Future release cần manual review, serving packaging tương thích và deployment decision riêng.

![Các version Model Registry đang chờ phê duyệt thủ công](/images/5-Workshop/current/model-registry-agent-risk-scorer-v1-v2-pending.png)

*Hình 2. Model package versions `/1` và `/2` đã `Completed` nhưng vẫn ở trạng thái `PendingManualApproval`.*

## External/OOD không thay đổi governance state

External/OOD pilot 40 mẫu đánh giá frozen model tại local. Pilot không start Pipeline này, register package mới, approve hoặc deploy `/1` hay `/2`, tune decision threshold, cập nhật historical Endpoint hoặc thay thế Model Monitor baseline 854 rows. Mọi lần retraining tương lai chỉ nên bắt đầu sau khi có dữ liệu đại diện và independent human labels, sau đó phải đi qua cùng governed evaluation và manual release boundary.
