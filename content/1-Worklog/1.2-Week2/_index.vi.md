---
title: "Tuần 2: Xác định bài toán và đề xuất dự án"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 1.2. </b> "
---

## 08/06/2026 - 14/06/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

## Mục tiêu

Chuyển đề tài đã chọn thành proposal MVP cụ thể với problem statement, scope, workflow, API response dự kiến và trách nhiệm của từng AWS service.

## Bối cảnh

Tuần này tập trung làm cho project có thể đo lường được. Thay vì chỉ nói AI agent an toàn hay không an toàn, proposal cần xác định dữ liệu nào sẽ được quan sát và hệ thống sẽ trả về quyết định gì.

## AWS services và kiến thức đã tìm hiểu

Tuần này tập trung vào việc hiểu một project machine learning trên AWS nên được thiết kế như thế nào trước khi viết proposal. Tôi tìm hiểu vai trò của từng service và cách chúng kết hợp trong workflow từ data đến inference.

- **Amazon S3:** Tìm hiểu S3 như storage layer bền vững cho raw input data, processed datasets, model artifacts và demo evidence.
- **Amazon SageMaker Processing:** Rà soát vai trò của service này trong việc chạy data preprocessing và feature engineering scripts trên môi trường managed.
- **SageMaker Training:** Tìm hiểu managed training workflow dự kiến, gồm training containers, input channels, output artifacts, instance types và quotas.
- **SageMaker Endpoint:** Học rằng endpoint được dùng cho real-time inference và cần SageMaker Model cùng Endpoint Configuration.
- **AWS Lambda:** Tìm hiểu Lambda như serverless compute layer để transform API requests và gọi SageMaker Runtime.
- **Amazon API Gateway:** Rà soát cách HTTP API expose route rõ ràng như `POST /score-agent-run`.
- **CloudWatch và IAM:** Tìm hiểu vai trò hỗ trợ: CloudWatch cho logs và IAM cho least-privilege access giữa services.

Phần học AWS này được chuyển thành proposal architecture, trong đó mỗi AWS service có trách nhiệm rõ ràng thay vì chỉ được liệt kê như tên công nghệ.

## Bảng công việc theo ngày

| Ngày | Công việc đã thực hiện |
|---|---|
| 08/06/2026 | Xác định bài toán project: đánh giá AI coding-agent runs bằng trajectory evidence thay vì chỉ dựa vào final summary. |
| 09/06/2026 | Thiết kế các input field dự kiến cho files, commands, tests, lint checks, sensitive access và risky behavior. |
| 10/06/2026 | Phác thảo scoring response gồm risk_score, quality_score, predicted_label và decision. |
| 11/06/2026 | Mapping các giai đoạn project với AWS services và tách phạm vi MVP đã triển khai khỏi hướng MLOps mở rộng. |
| 12/06/2026 | Đọc tài liệu SageMaker Processing, S3, Lambda và API Gateway để kiểm tra tính hợp lý của architecture. |
| 13/06/2026 - 14/06/2026 | Hoàn thiện proposal evidence, chụp ảnh proposal và chuẩn bị bằng chứng Worklog Week 2. |


## Công việc kỹ thuật

- Định nghĩa trajectory logs là nguồn bằng chứng chính, gồm file đã đọc, file đã sửa, command đã chạy, test results, lint results, diff size, sensitive file access và risky command signals.
- Thiết kế response schema gồm risk_score, quality_score, predicted_label và decision để output hỗ trợ các quyết định allow, require_review hoặc block.
- Mapping từng giai đoạn project với AWS service: S3 cho raw/processed data và model artifacts, SageMaker Processing cho feature engineering, SageMaker Endpoint cho inference, Lambda xử lý request, API Gateway tạo HTTP route, CloudWatch lưu log và IAM kiểm soát quyền.
- Xác định giới hạn trung thực rằng có thể thử SageMaker Training, nhưng local XGBoost training là fallback hợp lý nếu account quota chặn managed training.

## Deliverables

- **Hoàn thành problem statement.**
- **Viết phạm vi MVP.**
- **Phác thảo API request/response.**
- **Xác định trách nhiệm của từng thành phần AWS architecture.**

## Khó khăn và cách xử lý

**Khó khăn:** Bài toán risk scoring có thể trở nên quá rộng nếu bao gồm đầy đủ production MLOps, policy engine và real-time monitoring ngay từ đầu.

**Cách xử lý:** Proposal giới hạn phiên bản đầu ở luồng end-to-end thực tế từ logs đến scoring API, đồng thời ghi Model Registry, Model Monitor và Pipelines là hướng mở rộng.

## Liên hệ với project chính

Tuần này đóng góp vào MVP cuối bằng cách củng cố luồng từ **bằng chứng hành vi của AI coding agent** đến **workflow đánh giá rủi ro trên AWS**. Nội dung giúp workshop cuối không chỉ là giải thích khái niệm, mà còn bám theo đúng trình tự triển khai thực tế của project.

## Ảnh bằng chứng

![Project proposal phần 1](/images/worklog/week02-project-proposal_1.png)

![Project proposal phần 2](/images/worklog/week02-project-proposal_2.png)

Vì nội dung proposal khá dài, file Markdown đầy đủ cũng được đính kèm tại đây: [week02-project-proposal.md](/images/worklog/week02-project-proposal.md).

## Bằng chứng và tài liệu tham khảo đã tìm hiểu

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [SageMaker Processing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)
- [Amazon API Gateway Developer Guide](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)

---

[Trước](/vi/1-worklog/1.1-week1/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.3-week3/)
