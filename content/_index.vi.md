---
title: "Báo cáo thực tập"
date: 2024-01-01
weight: 1
chapter: false
---

# Báo cáo thực tập

## Thông tin sinh viên

<img src="/images/student-avatar.png" alt="Chu Nguyễn Tuấn Anh" width="220">

| Mục | Thông tin |
|---|---|
| Họ tên | Chu Nguyễn Tuấn Anh |
| Số điện thoại | 0962037357 |
| Email | anh.chunguyentuan@hcmut.edu.vn |
| Trường | Ho Chi Minh City University of Technology , HCMUT |
| Chuyên ngành | Computer Science |
| Công ty thực tập | Amazon Web Services Viet Nam Company Limited |
| Vị trí thực tập | Workforce Bootcamp - First Cloud AI Journey |
| Thời gian thực tập | 01/06/2026-23/08/2026 |

## Tên đề tài

**Xây dựng và triển khai hệ thống đánh giá chất lượng và rủi ro cho AI Coding Agent trên AWS SageMaker**

Báo cáo này trình bày một workflow Machine Learning/MLOps đã hoàn thành trên AWS để đánh giá các lần chạy AI Coding Agent từ trajectory logs: Amazon S3, SageMaker Processing, managed XGBoost Training, held-out Evaluation, Experiments/HPO, Pipeline, Model Registry, historical serving ngắn hạn, Data Capture, Model Monitor và CloudWatch acceptance.

Một External/OOD diagnostic local độc lập đã đánh giá frozen model trên 40 public trajectories được pin revision mà không retrain hoặc gọi AWS. Synthetic macro F1 `1.00` giảm xuống external macro F1 `0.1212`, cho thấy generalization gap đáng kể. Xem [Workshop](/vi/5-workshop/) để đọc technical evidence và [Tự đánh giá](/vi/6-self-evaluation/) để xem bài học cùng hạn chế.

## Cấu trúc báo cáo

1. [Nhật ký công việc](/vi/1-worklog/)
2. [Đề xuất dự án](/vi/2-proposal/)
3. [Các bài blog đã đăng](/vi/3-blogsposted/)
4. [Các sự kiện đã tham gia](/vi/4-eventparticipated/)
5. [Workshop](/vi/5-workshop/)
6. [Tự đánh giá](/vi/6-self-evaluation/)
7. [Chia sẻ và phản hồi](/vi/7-feedback/)
8. [Tài liệu tham khảo](/vi/8-references/)
