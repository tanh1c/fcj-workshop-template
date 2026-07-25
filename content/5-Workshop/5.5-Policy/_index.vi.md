---
title: "Chạy SageMaker Processing"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5.5. </b> "
---

SageMaker Processing áp dụng cùng feature extraction như local preview và ghi `train.csv`, `validation.csv`, `test.csv` lên S3.

## Local preview miễn phí

```bash
python preprocessing/processing_script.py \
  --input data_generation/combined_trajectories.jsonl \
  --output-dir data/processed \
  --seed 42
```

Kiểm tra ba CSV files và thứ tự columns trước mọi AWS job.

## Optional managed Processing

Command này tạo Processing Job có phí. Chỉ chạy sau confirmation gate trong [Chuẩn bị](/vi/5-workshop/5.2-prerequiste/).

```bash
python preprocessing/run_sagemaker_processing.py \
  --bucket "<processing-bucket>" \
  --role-arn "<sagemaker-execution-role-arn>" \
  --region ap-southeast-1 \
  --instance-type ml.t3.medium \
  --input data_generation/combined_trajectories.jsonl
```

## Evidence đã nghiệm thu

Historical job `agent-risk-processing-1782829845` hoàn tất tại `ap-southeast-1` và tạo ba CSV splits dưới processed S3 prefix. Hiện không có Processing Job đang chạy. Pipeline sau đó thực hiện lại preprocessing tại `us-east-1` trong governed execution.

Không chạy lại Processing chỉ để tái tạo evidence; hãy xem retained CSV artifacts và accepted job record.
