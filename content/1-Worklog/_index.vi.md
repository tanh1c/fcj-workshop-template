---
title: "Nhật ký công việc"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 1. </b> "
---

## Tổng quan nhật ký công việc

Worklog này bao quát kỳ thực tập 12 tuần từ **01/06/2026 đến 23/08/2026**. Nội dung hiện được cập nhật đến **24/07/2026**: Tuần 1–7 là nhật ký lịch sử, Tuần 8 đang diễn ra với accepted AWS evidence đến ngày 23/07, còn Tuần 9–12 là kế hoạch chưa hoàn tất.

Công việc được thực hiện kết hợp giữa **triển khai cá nhân** và **học tập/thảo luận theo nhóm** trong chương trình **Workforce Bootcamp - First Cloud AI Journey**. Chương trình **không có mentor cố định**, nên worklog được viết như một bản ghi chuyên nghiệp tự quản lý, gồm mục tiêu theo tuần, công việc kỹ thuật, deliverables, khó khăn, cách xử lý và tài liệu tham khảo đã học.

## Điều hướng theo tuần

1. [Tuần 1: Onboarding và nền tảng AWS](/vi/1-worklog/1.1-week1/)
2. [Tuần 2: Xác định bài toán và đề xuất dự án](/vi/1-worklog/1.2-week2/)
3. [Tuần 3: Thiết kế dataset và mô phỏng trajectory logs](/vi/1-worklog/1.3-week3/)
4. [Tuần 4: Chuẩn bị S3 data layout và IAM](/vi/1-worklog/1.4-week4/)
5. [Tuần 5: SageMaker Processing và feature engineering](/vi/1-worklog/1.5-week5/)
6. [Tuần 6: Training và đánh giá XGBoost](/vi/1-worklog/1.6-week6/)
7. [Tuần 7: Đóng gói model và chuẩn bị managed governance](/vi/1-worklog/1.7-week7/)
8. [Tuần 8: Managed ML governance và accepted evidence](/vi/1-worklog/1.8-week8/) — đang thực hiện đến 23/07/2026
9. [Tuần 9: Kế hoạch kiểm tra end-to-end](/vi/1-worklog/1.9-week9/) — kế hoạch
10. [Tuần 10: Kế hoạch review monitoring và chi phí](/vi/1-worklog/1.10-week10/) — kế hoạch
11. [Tuần 11: Kế hoạch hoàn thiện báo cáo](/vi/1-worklog/1.11-week11/) — kế hoạch
12. [Tuần 12: Kế hoạch nộp bài và bàn giao](/vi/1-worklog/1.12-week12/) — kế hoạch

## Định hướng project chính

Các tuần làm việc tập trung vào project **Xây dựng và triển khai hệ thống đánh giá chất lượng và rủi ro cho AI Coding Agent trên AWS SageMaker**. Evidence đã hoàn tất được tách thành hai track: managed Training, evaluation, HPO, Pipeline, Registry và monitoring acceptance tại `us-east-1`; cùng historical serving ngắn hạn qua SageMaker Endpoint, Lambda, API Gateway, Data Capture và CloudWatch tại `ap-southeast-1`. Managed Registry packages vẫn là `PendingManualApproval` và chưa được deploy.

## Cấu trúc mỗi tuần

Mỗi trang tuần bao gồm:

- **Mục tiêu**: tuần đó cần đạt được điều gì.
- **Bối cảnh**: vì sao công việc đó quan trọng trong timeline project.
- **Công việc kỹ thuật**: các bước nghiên cứu hoặc triển khai cụ thể.
- **Deliverables**: artifact hoặc kết quả tạo ra trong tuần.
- **Khó khăn và cách xử lý**: vấn đề thực tế gặp phải và hướng giải quyết.
- **Bằng chứng và tài liệu tham khảo**: AWS documentation, tutorial hoặc reference kỹ thuật đã tìm hiểu.
