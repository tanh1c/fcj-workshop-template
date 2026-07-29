---
title: "Tuần 9: Kế hoạch validation end-to-end và nhận feedback"
date: 2024-01-01
weight: 9
chapter: false
pre: " <b> 1.9. </b> "
---

## 27/07/2026 - 02/08/2026

**Trạng thái:** Kế hoạch  
**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.  
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.

## Mục tiêu

Validation behavior end-to-end đã document bằng local checks và accepted evidence, sau đó thu thập reviewer feedback mà không rerun AWS resources có phí.

## Công việc dự kiến

- Validate local schema và generated trajectories mà không tạo AWS mutation.
- Review accepted historical API, Data Capture và score-response evidence thay vì rerun paid resources.
- Thu thập reviewer feedback về human-review và hard-rule explanations.
- Sửa documentation inconsistencies và ghi lại feedback nhận được.
- Chỉ dùng live short-lived serving demo nếu có confirmation riêng; nếu không thì dùng accepted evidence.

## Kế hoạch theo ngày

| Ngày | Công việc dự kiến |
|---|---|
| 27/07/2026 | Validate local trajectory schema, labels và feature ordering. |
| 28/07/2026 | Review accepted Endpoint/API response và Data Capture evidence. |
| 29/07/2026 | So sánh decisions với trajectory evidence và hard safety rules. |
| 30/07/2026 | Review local các scenario safe, failed, risky và require-review. |
| 31/07/2026 | Thu thập reviewer feedback về evidence và decision explanations. |
| 01/08/2026 - 02/08/2026 | Áp dụng documentation corrections và ghi feedback nhận được. |

## Deliverables dự kiến

- **Local schema và trajectory checks.**
- **Review accepted end-to-end evidence.**
- **Reviewer feedback record.**
- **Documentation corrections nếu cần.**

Chưa claim evidence Tuần 9 tại thời điểm 25/07/2026. Mọi live AWS serving action cần confirmation riêng; accepted historical evidence là default path.

---

[Trước](/vi/1-worklog/1.8-week8/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.10-week10/)
