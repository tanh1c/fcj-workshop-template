---
title: "Tuần 1: Onboarding và nền tảng AWS"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1.1. </b> "
---

## 01/06/2026 - 07/06/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

## Mục tiêu

Hiểu yêu cầu thực tập, thiết lập kế hoạch làm việc và xây dựng nền tảng AWS cần thiết trước khi chọn và triển khai project kỹ thuật.

## Bối cảnh

Tuần đầu tiên chủ yếu dùng để chuyển từ bối cảnh học bootcamp tổng quát sang một định hướng thực tập cụ thể. Vì chương trình không có mentor cố định, cách làm việc cần được tự quản lý: xác định mục tiêu theo tuần, lưu bằng chứng và luôn bám yêu cầu báo cáo cuối kỳ ngay từ đầu.

## AWS services và kiến thức đã tìm hiểu

Trước khi chọn hướng triển khai project, tôi dành tuần này để xây nền tảng về các khái niệm AWS cốt lõi. Mục tiêu là hiểu mỗi AWS service chịu trách nhiệm gì trước khi đưa chúng vào MVP cuối.

- **AWS Global Infrastructure:** Tìm hiểu Region và Availability Zone để hiểu vì sao project nên dùng nhất quán một Region, đặc biệt là `ap-southeast-1`, cho S3, SageMaker, Lambda và API Gateway.
- **AWS Identity and Access Management (IAM):** Rà soát users, roles, policies, trust relationships và nguyên tắc least privilege. Phần này quan trọng vì project về sau cần quyền riêng cho SageMaker và Lambda.
- **Shared Responsibility Model:** Tìm hiểu phần trách nhiệm thuộc về AWS và phần thuộc về người dùng, nhất là credentials, IAM permissions, data access và cleanup resources.
- **Amazon S3 concept:** Tìm hiểu buckets, objects, prefixes và use case lưu trữ để sau này tổ chức data thành raw, processed và model artifact locations.
- **Amazon CloudWatch overview:** Rà soát CloudWatch như service dùng cho logs và operational visibility khi AWS managed jobs hoặc functions chạy.
- **Initial AWS service mapping:** Liên kết project idea với service chain ban đầu: S3 cho storage, SageMaker cho ML workflow, Lambda/API Gateway cho API exposure, CloudWatch cho logs và IAM cho access control.

Sau bước học này, tôi có đủ ngữ cảnh AWS để chọn project scope phù hợp với tài khoản AWS sinh viên và yêu cầu báo cáo thực tập.

## Bảng công việc theo ngày

| Ngày | Công việc đã thực hiện |
|---|---|
| 01/06/2026 | Rà soát yêu cầu thực tập và xác định toàn bộ các mục cần có trong báo cáo. |
| 02/06/2026 | Tìm hiểu nền tảng AWS account, global infrastructure, Regions và Availability Zones. |
| 03/06/2026 | Ôn IAM, shared responsibility và trách nhiệm bảo mật trong project AWS sinh viên. |
| 04/06/2026 | So sánh các hướng project và chọn AI Coding Agent risk scoring làm hướng chính. |
| 05/06/2026 | Phác thảo service map MVP ban đầu với S3, SageMaker, Lambda, API Gateway, CloudWatch và IAM. |
| 06/06/2026 - 07/06/2026 | Tổ chức ghi chú, reference và evidence ban đầu cho Worklog và báo cáo cuối kỳ. |


## Công việc kỹ thuật

- Rà soát cấu trúc báo cáo yêu cầu và xác định output cuối cần có worklog, proposal, blogs, events, workshop, self-evaluation và feedback.
- Ôn nền tảng AWS như Regions, Availability Zones, IAM, shared responsibility và sự khác nhau giữa storage, compute, networking, managed ML services.
- So sánh các hướng project và chọn AI Coding Agent risk scoring vì kết hợp AI safety, software engineering evidence và AWS ML deployment.
- Phác thảo service chain dự kiến: S3 lưu dữ liệu, SageMaker xử lý/host model, Lambda và API Gateway expose API, CloudWatch lưu log, IAM quản lý quyền.

## Deliverables

- **Chọn được đề tài thực tập.**
- **Tạo outline báo cáo ban đầu.**
- **Xác định phạm vi học AWS.**
- **Phác thảo service map MVP mức cao.**

## Khó khăn và cách xử lý

**Khó khăn:** Khó khăn chính là thu hẹp phạm vi AI/cloud rất rộng thành một project đủ thực tế với AWS account sinh viên.

**Cách xử lý:** Phạm vi được giới hạn thành MVP trung thực: chấm điểm agent runs từ trajectory logs, demo AWS workflow và tách rõ phần đã triển khai với phần MLOps mở rộng trong tương lai.

## Liên hệ với project chính

Tuần này đóng góp vào MVP cuối bằng cách củng cố luồng từ **bằng chứng hành vi của AI coding agent** đến **workflow đánh giá rủi ro trên AWS**. Nội dung giúp workshop cuối không chỉ là giải thích khái niệm, mà còn bám theo đúng trình tự triển khai thực tế của project.

## Ảnh bằng chứng

![Trang học AWS Cloud Practitioner](/images/worklog/week01-aws-learning.png)

Ảnh chụp thể hiện trang học AWS Cloud Practitioner được dùng làm điểm bắt đầu cho phần nền tảng AWS.

## Bằng chứng và tài liệu tham khảo đã tìm hiểu

- [AWS Cloud Practitioner Essentials](https://aws.amazon.com/training/digital/aws-cloud-practitioner-essentials/)
- [AWS Well-Architected Framework](https://docs.aws.amazon.com/wellarchitected/latest/framework/welcome.html)
- [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

---

[Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.2-week2/)
