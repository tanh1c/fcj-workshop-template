---
title: "Tuần 4: Chuẩn bị S3 data layout và IAM"
date: 2024-01-01
weight: 4
chapter: false
pre: " <b> 1.4. </b> "
---

## 22/06/2026 - 28/06/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

## Mục tiêu

Chuẩn bị nền tảng storage và permission trên AWS để di chuyển dữ liệu và artifacts trong MVP một cách an toàn.

## Bối cảnh

Trước khi chạy SageMaker jobs, project cần layout S3 và kế hoạch IAM rõ ràng. Điều này giảm nhầm lẫn khi raw logs, processed CSV files và model artifacts di chuyển giữa local development, SageMaker, Lambda và endpoint.

## AWS services và kiến thức đã tìm hiểu

Tuần này tập trung vào Amazon S3 và IAM vì đây là nền tảng cho hầu hết các AWS services phía sau trong project. Trước khi chạy SageMaker Processing, tôi cần hiểu cách tổ chức data paths và permissions.

- **Amazon S3 bucket usage:** Tìm hiểu cách một bucket có thể lưu nhiều vùng dữ liệu của project bằng prefixes thay vì folder vật lý riêng biệt.
- **S3 prefix design:** Lên layout project với `raw/` cho JSONL logs, `processed/` cho CSV outputs và `models/` cho model artifacts.
- **AWS CLI operations:** Thực hành hoặc rà soát lệnh list buckets, upload files, kiểm tra object paths và verify outputs mong muốn trong S3.
- **S3 URI format:** Học cách các path như `s3://bucket/prefix/file` được truyền vào SageMaker jobs và deployment scripts.
- **IAM execution role:** Tìm hiểu vì sao SageMaker cần execution role để đọc input data từ S3 và ghi output data trở lại.
- **Least-privilege access:** Rà soát lý do role chỉ nên có quyền cần thiết cho workflow, thay vì cấp quyền administrator quá rộng.
- **Evidence collection:** Dùng screenshots S3 và object paths làm evidence bền vững vì S3 artifacts vẫn tồn tại sau khi temporary compute resources được cleanup.

Phần học AWS này giúp workflow project lặp lại được tốt hơn vì các bước processing, training và deployment phía sau đều phụ thuộc vào S3 paths ổn định và IAM permissions đúng.

## Bảng công việc theo ngày

| Ngày | Công việc đã thực hiện |
|---|---|
| 22/06/2026 | Lập kế hoạch S3 prefix structure cho raw logs, processed datasets và model artifacts. |
| 23/06/2026 | Tạo hoặc kiểm tra project S3 bucket và xác nhận folder layout dự kiến. |
| 24/06/2026 | Rà soát yêu cầu SageMaker execution role để đọc input data và ghi processed outputs. |
| 25/06/2026 | Rà soát yêu cầu Lambda role cho CloudWatch logging và SageMaker Runtime invocation. |
| 26/06/2026 | Chuẩn bị AWS CLI commands để kiểm tra S3 paths và thu thập evidence có thể lặp lại. |
| 27/06/2026 - 28/06/2026 | Chụp ảnh S3/IAM và ghi lại quy ước đặt tên resources. |


## Công việc kỹ thuật

- Lập kế hoạch S3 prefixes cho raw trajectory logs, processed train/validation/test datasets và trained model artifacts.
- Rà soát least-privilege access patterns cho SageMaker execution roles, Lambda execution roles, quyền S3 read/write và quyền invoke SageMaker Runtime.
- Chuẩn bị AWS CLI commands để upload raw JSONL logs, liệt kê output và kiểm tra vị trí artifacts.
- Ghi lại environment details dùng về sau trong báo cáo, gồm region, cách đặt tên bucket theo account, role naming và yêu cầu cleanup resources.

## Deliverables

- **Chuẩn bị S3 data layout.**
- **Làm rõ trách nhiệm IAM roles.**
- **Phác thảo AWS CLI workflow.**
- **Ghi lại convention đặt tên resources.**

## Khó khăn và cách xử lý

**Khó khăn:** IAM trong demo ban đầu dễ bị quá rộng quyền hoặc quá chặt khiến workflow không chạy được.

**Cách xử lý:** Quyền được phân tích theo từng service: SageMaker cần S3 access cho processing và model artifacts, Lambda cần SageMaker Runtime access, còn CloudWatch logging cần cho debugging.

## Liên hệ với project chính

Tuần này đóng góp vào MVP cuối bằng cách củng cố luồng từ **bằng chứng hành vi của AI coding agent** đến **workflow đánh giá rủi ro trên AWS**. Nội dung giúp workshop cuối không chỉ là giải thích khái niệm, mà còn bám theo đúng trình tự triển khai thực tế của project.

## Ảnh bằng chứng

![S3 bucket layout cho dữ liệu project](/images/worklog/week04-s3-layout.png)

![IAM role dùng trong AWS workflow](/images/worklog/week04-iam-role.png)

Các ảnh chụp thể hiện nền tảng lưu trữ và phân quyền được chuẩn bị trước khi chạy workflow xử lý trên AWS.

## Bằng chứng và tài liệu tham khảo đã tìm hiểu

- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [IAM roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles.html)
- [AWS CLI Command Reference](https://docs.aws.amazon.com/cli/latest/reference/)

---

[Trước](/vi/1-worklog/1.3-week3/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.5-week5/)
