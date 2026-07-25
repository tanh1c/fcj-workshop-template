---
title: "Tuần 7: Đóng gói model và chuẩn bị managed governance"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---

## 13/07/2026 - 19/07/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.  
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.  
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

## Mục tiêu

Chuẩn bị model packaging tương thích SageMaker và managed ML workflow có governance trước khi chạy các AWS jobs được nghiệm thu.

## Bối cảnh

Tuần 6 tạo local fallback artifact khi Region ban đầu chưa có Training quota. Tuần 7 tập trung vào packaging, held-out evaluation, HPO, Pipeline, Registry governance và historical-serving runbook có kiểm soát chi phí. Tuần này không claim rằng managed Registry package đã được deploy.

> **Chuẩn bị, không phải Registry deployment:** Không managed Registry package nào được approve hoặc deploy trong Tuần 7. Accepted AWS runs và historical serving evidence tách biệt được ghi ở Tuần 8.

## AWS services và kiến thức đã tìm hiểu

- **Model packaging:** Đóng gói lại XGBoost artifacts cùng inference code và decision-policy code tương thích.
- **Held-out evaluation:** Chuẩn bị đánh giá trên test split riêng với safety-oriented metrics.
- **SageMaker Experiments và HPO:** Chuẩn bị bounded Random tuning run với ba child jobs chạy tuần tự.
- **SageMaker Pipeline:** Compile workflow local và verify `Preprocess → Train → Evaluate → CheckRiskyRecall`.
- **Safety gate:** Định nghĩa `risky_recall >= 0.85` là điều kiện cho phép registration, không phải approval để deploy.
- **Model Registry:** Giữ package approval ở `PendingManualApproval` để registration và release là hai quyết định riêng.
- **Historical serving:** Chuẩn bị runbook Endpoint/Lambda/API ngắn hạn với confirmation và cleanup bắt buộc.

## Bảng công việc theo ngày

| Ngày | Công việc đã thực hiện |
|---|---|
| 13/07/2026 | Đóng gói lại managed-compatible XGBoost artifact và inference code. |
| 14/07/2026 | Chuẩn bị held-out evaluation inputs và safety-oriented metric checks. |
| 15/07/2026 | Chuẩn bị SageMaker Experiments và bounded Random HPO configuration. |
| 16/07/2026 | Compile Pipeline local và review conditional registration graph. |
| 17/07/2026 | Verify risky-recall gate chỉ cho phép registration và giữ approval thủ công. |
| 18/07/2026 - 19/07/2026 | Chuẩn bị historical-serving confirmation gate, cleanup order và evidence checklist. |

## Công việc kỹ thuật

- Đóng gói lại managed-compatible XGBoost artifact và inference code.
- Chuẩn bị held-out evaluation, HPO, Pipeline và Registry scripts/configuration.
- Compile Pipeline local và verify cấu trúc `Preprocess → Train → Evaluate → CheckRiskyRecall`.
- Định nghĩa release boundary: `risky_recall >= 0.85` chỉ cho phép registration; approval và deployment vẫn là quyết định thủ công.
- Chuẩn bị short-lived historical-serving runbook với paid-resource confirmation và cleanup order rõ ràng.

## Deliverables

- **Managed-compatible artifact và inference package đã chuẩn bị.**
- **Held-out evaluation và bounded HPO configuration đã chuẩn bị.**
- **Pipeline đã compile local với conditional safety gate.**
- **Registry approval boundary được ghi là `PendingManualApproval`.**
- **Historical-serving confirmation và cleanup runbook đã chuẩn bị.**

## Khó khăn và cách xử lý

**Khó khăn:** Packaging, registration, approval và deployment là các lifecycle stage riêng, nhưng báo cáo đơn giản hóa có thể vô tình trình bày chúng như một hành động đã hoàn tất.

**Cách xử lý:** Tuần này chỉ ghi preparation. Accepted managed jobs, registration outcomes và historical serving được ghi ở Tuần 8, đồng thời giữ managed artifact và serving artifact thành hai evidence track riêng.

## Liên hệ với project chính

Tuần này thiết lập governance boundary cho project hoàn thiện: metric thành công có thể cho phép registration, còn human review kiểm soát approval và deployment.

## Evidence lịch sử

![Local model artifact trước đó được giữ trong S3](/images/worklog/week07-model-artifact-s3.png)

Ảnh này ghi lại việc lưu earlier local artifact, artifact sau đó được dùng cho historical serving demo riêng. Đây không phải evidence cho thấy managed Registry package nào đã được deploy.

## Bằng chứng và tài liệu tham khảo đã tìm hiểu

- [SageMaker Pipelines](https://docs.aws.amazon.com/sagemaker/latest/dg/pipelines.html)
- [SageMaker Model Registry](https://docs.aws.amazon.com/sagemaker/latest/dg/model-registry.html)
- [Automatic Model Tuning](https://docs.aws.amazon.com/sagemaker/latest/dg/automatic-model-tuning.html)

---

[Trước](/vi/1-worklog/1.6-week6/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.8-week8/)
