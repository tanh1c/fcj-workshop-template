---
title: "Tuần 6: Training và đánh giá XGBoost"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 1.6. </b> "
---

## 06/07/2026 - 12/07/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

## Mục tiêu

Train scoring model đầu tiên và đánh giá bằng các metric ưu tiên phát hiện risky-run và hallucinated-success.

## Bối cảnh

Sau feature engineering, project cần supervised model để chuyển tabular behavior features thành decision signal. XGBoost được chọn vì phù hợp với structured tabular data và được SageMaker hỗ trợ.

## AWS services và kiến thức đã tìm hiểu

Tuần này tập trung vào khái niệm model training trên AWS và giới hạn thực tế của student-account quotas. Tôi tìm hiểu SageMaker Training workflow dự kiến trước khi quyết định dùng local XGBoost fallback.

- **SageMaker Training purpose:** Học rằng SageMaker Training chạy model training jobs trên managed infrastructure và ghi trained artifacts trở lại S3.
- **Built-in XGBoost:** Rà soát SageMaker support cho XGBoost và lý do nó phù hợp với tabular classification tasks.
- **Training input channels:** Tìm hiểu cách training và validation CSV files từ S3 được truyền vào training job.
- **Training output artifact:** Rà soát cách một completed training job thường tạo model artifacts lưu trong S3.
- **Instance type selection:** Học rằng training jobs cần ML instance quotas khả dụng, có thể khác nhau theo account và Region.
- **AWS Service Quotas:** Kiểm tra quota limitation và ghi nhận tài khoản sinh viên không chạy được managed training job dự kiến.
- **Local fallback:** Dùng local XGBoost training để project tiếp tục được, đồng thời document SageMaker Training là planned nhưng unavailable.
- **Evaluation focus:** Tìm hiểu vì sao safety-oriented metrics như risky recall và false-negative rate có ý nghĩa hơn accuracy đơn thuần trong risk-scoring project.

Tuần này thể hiện rằng tôi đã tìm hiểu managed training path của AWS trước, sau đó ra quyết định triển khai dựa trên ràng buộc thật của account thay vì trình bày sai kết quả.

> **Trạng thái lịch sử của tuần này:** Trong Tuần 6, quota Training tại `ap-southeast-1` chưa khả dụng nên local XGBoost được dùng để tiếp tục project. Sau khi quota `ml.m5.large` riêng được duyệt tại `us-east-1`, managed Training hoàn tất ở Tuần 8. Các ảnh dưới đây vẫn là evidence đúng của quyết định trong Tuần 6, không phải trạng thái cuối của managed lifecycle.

## Bảng công việc theo ngày

| Ngày | Công việc đã thực hiện |
|---|---|
| 06/07/2026 | Chuẩn bị processed CSV files cho model training và chọn XGBoost cho tabular classification. |
| 07/07/2026 | Căn chỉnh training theo SageMaker Training và kiểm tra giới hạn quota của account. |
| 08/07/2026 | Dùng local XGBoost training làm fallback khi managed training quota không khả dụng. |
| 09/07/2026 | Tạo evaluation metrics gồm accuracy, macro F1, risky recall và các metric liên quan false negative. |
| 10/07/2026 | Đóng gói model outputs và verify folder model artifact. |
| 11/07/2026 - 12/07/2026 | Chụp evaluation, model artifact và quota screenshots để tài liệu MVP trung thực hơn. |


## Công việc kỹ thuật

- Chuẩn bị training data từ processed CSV files và chọn target labels cho classification.
- Thiết kế workflow theo SageMaker Training, đồng thời ghi rõ managed training quota bị chặn trong AWS account sinh viên.
- Dùng local XGBoost training làm fallback thực tế để project vẫn tạo được model artifact và tiếp tục deploy endpoint.
- Đánh giá accuracy, macro F1, risky recall, risky false-negative rate và hallucinated-success recall, nhấn mạnh false negatives vì bỏ sót hành vi rủi ro nghiêm trọng hơn việc yêu cầu review.

## Deliverables

- **Train XGBoost model local.**
- **Tạo evaluation report.**
- **Ghi rõ giới hạn quota Tuần 6; managed retry được chuyển sang tuần sau.**
- **Chuẩn bị model artifact để đóng gói.**

## Khó khăn và cách xử lý

**Khó khăn:** Quota của AWS account chặn SageMaker Training job dự kiến, nên project cần tránh trình bày sai rằng managed training đã hoàn tất.

**Cách xử lý:** Báo cáo giữ local training như fallback trung thực của Tuần 6. Managed SageMaker Training được ghi ở tuần sau, khi quota riêng tại `us-east-1` khả dụng và job thực sự hoàn tất.

## Liên hệ với project chính

Tuần này đóng góp vào MVP cuối bằng cách củng cố luồng từ **bằng chứng hành vi của AI coding agent** đến **workflow đánh giá rủi ro trên AWS**. Nội dung giúp workshop cuối không chỉ là giải thích khái niệm, mà còn bám theo đúng trình tự triển khai thực tế của project.

## Ảnh bằng chứng

![XGBoost evaluation report](/images/worklog/week06-xgboost-evaluation.png)

![Folder local model artifact](/images/worklog/week06-model-artifact-local.png)

![Bằng chứng SageMaker Training quota](/images/worklog/week06-sagemaker-training-quota.png)

Các ảnh chụp thể hiện kết quả đánh giá XGBoost local, model artifact đã tạo và bằng chứng SageMaker Training quota giải thích vì sao local training được dùng như fallback thực tế.

## Bằng chứng và tài liệu tham khảo đã tìm hiểu

- [XGBoost algorithm with SageMaker](https://docs.aws.amazon.com/sagemaker/latest/dg/xgboost.html)
- [SageMaker training jobs](https://docs.aws.amazon.com/sagemaker/latest/dg/how-it-works-training.html)

---

[Trước](/vi/1-worklog/1.5-week5/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.7-week7/)
