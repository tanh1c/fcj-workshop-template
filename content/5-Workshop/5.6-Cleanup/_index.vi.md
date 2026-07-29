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

![Managed XGBoost Training Job đã hoàn tất](/images/5-Workshop/current/training-job-agent-risk-xgboost-1784625353-completed.png)

*Hình 1. Managed Training Job `agent-risk-xgboost-1784625353` hoàn tất trên `ml.m5.large`.*

![Held-out evaluation report được giữ trên S3](/images/5-Workshop/current/s3-evaluation-report-agent-risk-xgboost-1784625353.png)

*Hình 2. S3 giữ held-out evaluation report của XGBoost job đã nghiệm thu.*

![Tổng quan bounded HPO job đã hoàn tất](/images/5-Workshop/current/hpo-agent-risk-hpo-1784643415-best-child-1.png)

*Hình 3. HPO job `agent-risk-hpo-1784643415` hoàn tất bounded search.*

![Các HPO child jobs và kết quả được chọn](/images/5-Workshop/current/hpo-agent-risk-hpo-1784643415-best-child-2.png)

*Hình 4. HPO evidence hiển thị tập child jobs đã hoàn tất và kết quả được chọn.*

![Chi tiết HPO child job tốt nhất](/images/5-Workshop/current/hpo-agent-risk-hpo-1784643415-best-child-3.png)

*Hình 5. Child `agent-risk-hpo-1784643415-001-59146c4e` được giữ làm best job.*

Perfect scores này đến từ labels chủ yếu synthetic và được tạo để dễ phân tách. Chúng xác minh managed workflow execution, không chứng minh production quality hoặc generalization. Model local trước đó chỉ còn liên quan như artifact dùng trong historical serving demo.

## External/OOD Generalization Pilot

Một diagnostic local độc lập kiểm tra khả năng frozen 17-feature model generalize ra ngoài internal dataset chủ yếu synthetic. Pilot lấy 20 trajectories từ mỗi public source được pin revision với seed `42`:

| Public source | Pinned revision | Số mẫu |
|---|---|---:|
| `nebius/SWE-agent-trajectories` | `68195a1450865274106246d0d0296a1d6807b88e` | 20 |
| `nebius/SWE-rebench-openhands-trajectories` | `35455389ab51bf5e2306bfd436ef72d0f98bf882` | 20 |

Hai annotator AI-assisted độc lập gán nhãn từng trajectory, sau đó adjudicator giải quyết bất đồng.

| Annotation coverage | Kết quả |
|---|---:|
| Selected | `40` |
| Full-axis A/B agreement | `3/40 = 7.5%` |
| Accepted trực tiếp | `3` |
| Adjudicated | `37` |
| Excluded | `0` |
| Pending | `0` |
| Final labels | `failed=28`, `safe=10`, `risky=2` |

{{% notice warning %}}
Đây là multi-agent/AI-assisted labels, không phải independent human ground truth.
{{% /notice %}}

### Kết quả frozen model

| Phạm vi | Accuracy | Macro F1 | Risky precision | Risky recall | Risky F1 | Risky FNR |
|---|---:|---:|---:|---:|---:|---:|
| Tổng thể, 40 mẫu | `0.0500` | `0.1212` | `1.0000` | `0.5000` | `0.6667` | `0.5000` |
| SWE-agent, 20 mẫu | `0.1000` | `0.1889` | `1.0000` | `1.0000` | `1.0000` | `0.0000` |
| OpenHands, 20 mẫu | `0.0000` | `0.0000` | `0.0000` | `0.0000` | `0.0000` | `1.0000` |

Chỉ hai mẫu có final label `risky`, vì vậy risky recall có Wilson 95% interval rộng `[0.0945, 0.9055]`. Kết quả này là diagnostic, không phải production estimate ổn định.

![Báo cáo tổng hợp External OOD pilot](/images/5-Workshop/current/local-external-ood-report-summary.png)

*Hình 6. Local report ghi nhận 40 mẫu, macro F1 `0.1212`, risky recall `0.50` và kết quả theo từng source.*

### Baseline và failure evidence

| Diagnostic | Số mẫu | Accuracy | Macro F1 | Risky recall | Risky FNR |
|---|---:|---:|---:|---:|---:|
| Rule-only baseline | 40 | `0.0250` | `0.1111` | `0.5000` | `0.5000` |
| Source-only internal diagnostic (`diagnostic_only`) | 183 | `0.2077` | `0.1105` | — | — |

Source-only result được đánh dấu rõ `diagnostic_only`; nó kiểm tra legacy source indicators có giải thích internal labels hay không và không phải external baseline. Redacted false-negative evidence gồm một mẫu OpenHands có true label `risky`, predicted label `require_review` và risky probability khoảng `0.00156`. Record được giữ chỉ liệt kê high-level risk flags cùng parser warnings; website không công bố raw public trajectory hoặc annotation package.

![Evidence risky false negative đã được redacted](/images/5-Workshop/current/local-external-ood-risky-false-negative-redacted.png)

*Hình 7. False-negative record được công bố đã redacted và chỉ chứa các diagnostic fields cần để giải thích prediction bị bỏ sót.*

Các giới hạn kỹ thuật bổ sung có ảnh hưởng trực tiếp:

- Cả ba legacy source flags đều bằng 0 với external sources chưa từng thấy.
- Parser dùng observable evidence và giữ unknown evidence theo hướng thận trọng, vì vậy missing fields cùng default values cần được review.
- Trusted LabelEncoder artifact được tạo bằng scikit-learn `0.24.1` nhưng load dưới `1.8.0`, tạo compatibility warning.

{{% notice warning %}}
Pilot này chạy local trên frozen managed artifact. Nó không retrain model, tune threshold, gọi SageMaker, đưa external data qua AWS Pipeline, thay đổi Registry packages, cập nhật historical Endpoint hoặc thay thế Model Monitor baseline.
{{% /notice %}}

Mức giảm từ synthetic macro F1 `1.00` xuống external macro F1 `0.1212` cho thấy distribution shift đáng kể. Trước mọi production claim, project cần dataset đại diện lớn hơn, independent human labeling, review parser/default-value behavior và một governed evaluation cycle mới.
