---
title: "Tổng quan workshop"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 5.1. </b> "
---

AI Coding Agent có thể đọc code, sửa file, chạy command, thực thi test và tóm tắt kết quả. Các khả năng này hữu ích, nhưng một câu trả lời cuối thuyết phục không chứng minh hành vi bên dưới an toàn hoặc chính xác.

## Bài toán và quyết định

Project đánh giá toàn bộ trajectory: file đã đọc/sửa, tool và command đã dùng, kích thước diff, evidence test/lint, truy cập file nhạy cảm, hành động phá hoại và mức độ hỗ trợ cho claim cuối.

Response điển hình:

```json
{
  "risk_score": 0.6003,
  "quality_score": 0.3997,
  "predicted_label": "failed",
  "decision": "require_review"
}
```

Score hỗ trợ reviewer, không thay thế quyết định con người. Hard rules xác định vẫn bảo vệ file nhạy cảm và chặn command phá hoại.

## Phạm vi đã hoàn thiện

Implementation đã nghiệm thu gồm:

1. Nguồn training deterministic từ simulator và SWE-bench Lite pseudo-trajectories, cùng Mini LLM Agent trajectories dùng cho demo.
2. Amazon S3 và SageMaker Processing với contract 17 features dùng chung.
3. Managed SageMaker XGBoost Training và held-out evaluation.
4. SageMaker Experiments và bounded Random HPO.
5. Pipeline gate `risky_recall >= 0.85` và conditional Model Registry registration.
6. Historical Endpoint, Lambda và API Gateway ngắn hạn.
7. Endpoint Data Capture, Lambda EMF, Model Monitor và CloudWatch acceptance.
8. Cleanup tài nguyên serving/monitoring trả phí nhưng giữ lại evidence.
9. Một External/OOD diagnostic local độc lập trên 40 public trajectories được pin revision bằng frozen 17-feature model.

## Split-Region và governance boundary

Project không thể chạy hoàn toàn tại `ap-southeast-1` vì quota SageMaker Training cần thiết không được cấp tại Region này. Quota cho `1 x ml.m5.large` được duyệt tại `us-east-1`, nên managed Training và governance workflow phụ thuộc được chuyển sang Region đó.

- `us-east-1`: managed Training, evaluation artifacts, Experiments/HPO, Pipeline, Model Registry và Model Monitor acceptance.
- `ap-southeast-1`: Processing/Studio lịch sử cùng Endpoint, Lambda, API Gateway và CloudWatch serving ngắn hạn.

Model packages `agent-risk-scorer/1` và `/2` vẫn là `PendingManualApproval`. Pipeline không approve hoặc deploy package nào. Historical Endpoint dùng artifact train local trước đó và không được trình bày như Registry deployment.

## Giới hạn evidence

Held-out metrics hoàn hảo vì dataset hiện tại chủ yếu synthetic và được tạo để dễ phân tách. Chúng xác minh managed workflow chạy đúng, không chứng minh production model quality hoặc real-world generalization. Cần trajectory thực và human labeling trước khi dùng production.
