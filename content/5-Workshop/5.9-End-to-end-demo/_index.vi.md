---
title: "Kiểm tra end-to-end và evidence"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 5.9. </b> "
---

Kiểm tra workflow theo hướng evidence-first. Free local checks và retained AWS records là đủ; live serving stack là tùy chọn và cần authorization rõ ràng.

## Free local validation

```bash
PYTHONPATH=demo_repo pytest demo_repo/tests -v
pytest agent data_generation preprocessing training inference pipelines lambda monitoring -q

python preprocessing/processing_script.py \
  --input data_generation/combined_trajectories.jsonl \
  --output-dir data/processed \
  --seed 42
```

Xác nhận trajectory schema, thứ tự 17 features, labels, train/validation/test splits và local test results.

## Accepted AWS evidence checklist

- Processing Job hoàn tất và giữ train/validation/test CSV.
- Managed XGBoost Training Job hoàn tất trên `1 x ml.m5.large`.
- Held-out evaluation report có 183 rows và safety metrics.
- Random HPO hoàn tất ba child jobs và giữ selected metadata.
- Pipeline execution `z9y3p0bqaske` thành công qua conditional registration.
- Registry versions `/1` và `/2` vẫn `PendingManualApproval`, chưa deploy.
- Historical Endpoint và `POST /score-agent-run` trả HTTP `200` trước cleanup.
- S3 giữ JSON Data Capture evidence.
- Model Monitor ghi `CompletedWithViolations` và giữ reports.
- CloudWatch giữ metrics/logs; dashboard và bảy alarms đã nghiệm thu được cleanup.

## Optional live serving sequence

Sau confirmation gate:

1. Deploy một Endpoint `ml.t2.medium` ngắn hạn với Data Capture.
2. Direct invoke một lần.
3. Deploy Lambda và HTTP API.
4. Chỉ dùng URL mới được in ra cho một Mini Agent request.
5. Kiểm tra `runs/run_login_api.json` và `score_response`.
6. Cleanup API/Lambda trước, sau đó Endpoint resources.

Không chạy lại Processing, Training, HPO, Pipeline hoặc Model Monitor cho live sequence. Nếu Endpoint không đạt `InService`, dừng, cleanup partial resources và trình bày retained evidence. Không retry mù hoặc đổi sang instance lớn hơn.

## Diễn giải

Request thành công chứng minh integration của historical serving path. Nó không chứng minh managed Registry packages đã deploy, và perfect synthetic evaluation metrics không chứng minh production generalization.
