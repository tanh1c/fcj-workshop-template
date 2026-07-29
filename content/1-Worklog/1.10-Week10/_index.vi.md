---
title: "Tuần 10: Kế hoạch review monitoring và chi phí"
date: 2024-01-01
weight: 10
chapter: false
pre: " <b> 1.10. </b> "
---

## 03/08/2026 - 09/08/2026

**Trạng thái:** Kế hoạch  
**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.  
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.

## Mục tiêu

Review retained monitoring evidence, cost boundaries và cleanup state mà không recreate temporary AWS infrastructure.

## Công việc dự kiến

- Review retained Data Capture, `AgentRiskScorer` metrics, Model Monitor reports và cleanup evidence.
- Xác nhận không recreate paid Endpoint, monitoring schedule, Studio app, dashboard hoặc alarm để làm documentation.
- Review low-cost retained artifacts và IAM policies.
- Cập nhật cost/cleanup guidance nếu accepted evidence inventory thay đổi.
- Xem Data Capture cùng CloudWatch là durable path và Model Monitor là historical accepted evidence.

## Kế hoạch theo ngày

| Ngày | Công việc dự kiến |
|---|---|
| 03/08/2026 | Review retained Data Capture và `AgentRiskScorer` metrics. |
| 04/08/2026 | Review Model Monitor reports và documented limitations. |
| 05/08/2026 | Kiểm tra absence checklist cho temporary cost-bearing resources. |
| 06/08/2026 | Review retained S3 artifacts và IAM policies. |
| 07/08/2026 | Đối chiếu monitoring và cleanup guidance trong toàn báo cáo. |
| 08/08/2026 - 09/08/2026 | Chỉ cập nhật documentation nếu evidence inventory thay đổi. |

## Deliverables dự kiến

- **Review monitoring evidence.**
- **Review cost và absence checklist.**
- **Review retained artifacts và IAM.**
- **Cập nhật cleanup guidance nếu cần.**

Chưa claim kết quả hoặc evidence Tuần 10 tại thời điểm 25/07/2026. Kế hoạch này không cho phép tạo hoặc rerun AWS resource.

---

[Trước](/vi/1-worklog/1.9-week9/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.11-week11/)
