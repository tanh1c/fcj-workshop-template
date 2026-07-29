---
title: "Tuần 8: Managed ML governance và accepted evidence"
date: 2024-01-01
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---

## 20/07/2026 - 26/07/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.  
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.  
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

> **Trạng thái ngày 25/07/2026:** Tuần này vẫn đang diễn ra. Các mục hoàn tất dưới đây bao quát công việc từ 20–25/07; mục ngày 26/07 vẫn là kế hoạch và không được trình bày như đã hoàn thành.

## Mục tiêu

Hoàn tất và đối chiếu managed ML, governance, historical serving và observability evidence mà không tạo hiểu nhầm rằng registered model packages đã được deploy.

## Bối cảnh

Tuần này khép lại khoảng cách giữa local fallback trước đó và managed workflow đã hoàn tất. Managed Training, HPO, Pipeline, Registry và Model Monitor acceptance chạy tại `us-east-1`. Historical serving ngắn hạn, API, Data Capture và Lambda/CloudWatch evidence vẫn là track riêng tại `ap-southeast-1`, dùng earlier local artifact.

## Bảng công việc theo ngày

| Ngày | Trạng thái và công việc |
|---|---|
| 20/07/2026 | Hoàn tất managed XGBoost `1.7-1` Training tại `us-east-1` trên `1 × ml.m5.large` và đánh giá 183 held-out rows. |
| 21/07/2026 | Hoàn tất Random HPO gồm ba jobs chạy tuần tự, chọn best trial, đồng thời nghiệm thu governed Pipeline execution và Registry version `/2`. |
| 21/07/2026 | Nghiệm thu historical serving riêng tại `ap-southeast-1`, HTTP `200` scoring, 100% JSON Data Capture, `AgentRiskScorer` EMF, Model Monitor và CloudWatch evidence; sau đó cleanup temporary resources. |
| 22/07/2026 | Đối chiếu managed Registry versions `/1` và `/2` ở trạng thái `Completed` và `PendingManualApproval`; xác nhận không package nào được approve hoặc deploy. |
| 23/07/2026 | Cross-check reports, Workshop content, metrics, safety limitations và cleanup state. |
| 24/07/2026 | Hoàn tất đối chiếu final report và External/OOD pilot local có giới hạn với multi-agent A/B annotation, adjudication, frozen-model evaluation cùng report/demo updates. Không chạy paid AWS job hoặc serving resource nào. |
| 25/07/2026 | Tham dự trực tiếp AWS Vietnam Community Meetup tại AWS Hà Nội và ghi nhận bài học về AI-assisted engineering, open-source agent runtime, human review và AI-native infrastructure. |
| 26/07/2026 | **Kế hoạch:** Nhận reviewer feedback và chỉ sửa documentation; không cần rerun AWS. |

## Managed Training, evaluation và HPO

- Training job `agent-risk-xgboost-1784625353`: `Completed`, 140 billable seconds, XGBoost `1.7-1`, `1 × ml.m5.large`.
- Held-out evaluation: 183 rows, macro F1 `1.00`, risky recall `1.00`, risky false-negative rate `0.00`.
- HPO `agent-risk-hpo-1784643415`: Random strategy, ba child jobs hoàn tất theo tuần tự, chọn `agent-risk-hpo-1784643415-001-59146c4e`.

Labels synthetic được tạo để dễ phân tách giải thích perfect metrics. Kết quả này xác minh workflow execution, không chứng minh production generalization.

![Tóm tắt evidence managed Training, held-out evaluation và HPO](/images/worklog/week08-managed-training-hpo-evidence.svg)

## Pipeline và Model Registry governance

Pipeline execution `z9y3p0bqaske` thành công qua:

```text
Preprocess → Train → Evaluate → CheckRiskyRecall
```

Gate yêu cầu `risky_recall >= 0.85`; observed risky recall là `1.00`, nên conditional registration được cho phép. Registry versions `/1` và `/2` là `Completed` và `PendingManualApproval`. Không package nào được approve hoặc deploy, và Pipeline không có Endpoint deployment step.

![Tóm tắt evidence Pipeline safety gate và Model Registry](/images/worklog/week08-pipeline-registry-evidence.svg)

## Historical serving và request evidence

Temporary Endpoint tại `ap-southeast-1` dùng earlier local artifact, không dùng managed Registry package nào. `POST /score-agent-run` trả HTTP `200` với representative decision `require_review`. Endpoint Data Capture giữ 100% JSON input/output, còn Lambda phát Embedded Metric Format records dưới namespace `AgentRiskScorer`.

![Tóm tắt evidence historical serving, API, Data Capture và EMF](/images/worklog/week08-serving-observability-evidence.svg)

## Model Monitor, CloudWatch và cleanup

Accepted Model Monitor baseline có 854 rows và 17 features. One-time execution kết thúc với `CompletedWithViolations`, gồm drift ở `diff_total_lines` và `latency_total_ms`. Hai type findings bổ sung được giữ như hạn chế trung thực của boundary-valued demo traffic thay vì bị che giấu.

CloudWatch expose 101 Model Monitor data metrics. Dashboard và bảy actions-disabled alarms đã nghiệm thu dùng missing-data-safe behavior và được xóa sau verification. Temporary Endpoint, API, monitoring schedule, dashboard và alarm resources hiện không còn; reports, captures, logs và metrics vẫn được giữ làm evidence.

![Tóm tắt evidence Model Monitor, CloudWatch và cleanup](/images/worklog/week08-monitoring-cleanup-evidence.svg)

## External/OOD diagnostic — 24/07/2026

Pilot local lấy mẫu `20 + 20` public trajectories từ các revision được pin với seed `42`. Hai annotator AI-assisted độc lập có full-axis A/B agreement `3/40 = 7.5%`; 37 mẫu được adjudicate, không có mẫu excluded hoặc pending. Frozen model đạt external macro F1 `0.1212`, risky recall `0.5000` và risky false-negative rate `0.5000`.

Pilot không retrain model, tune threshold, gọi SageMaker hoặc đưa external data qua AWS Pipeline. Nó cho thấy generalization gap mà không thay đổi accepted AWS lifecycle hoặc cleanup state.

## Deliverables đến 25/07/2026

- **Managed Training và held-out evaluation đã nghiệm thu.**
- **Bounded HPO và selected best trial đã được ghi nhận.**
- **Governed Pipeline và hai Registry versions đã được đối chiếu.**
- **Historical serving/API và Data Capture evidence được giữ riêng.**
- **Model Monitor, CloudWatch và cleanup evidence đã được đối chiếu.**
- **Final report được đối chiếu với External/OOD pilot local và demo narrative.**
- **Multi-agent annotation/adjudication và frozen-model evaluation được ghi nhận mà không rerun AWS.**
- **Tham gia trực tiếp Event 4 và ghi nhận reflection về AI engineering, agent safety và AI-native infrastructure.**

## Decision boundary

Model score chỉ mang tính tư vấn. Human review và hard safety rules vẫn có thẩm quyền cao nhất, đặc biệt với destructive commands, sensitive paths, unsupported success claims và evidence không chắc chắn.

## Nguồn evidence

Bốn cards phía trên là static summaries tạo từ accepted local project records trong `report/demo_evidence.md` và `report/best_model_metrics.json`. Chúng không phải AWS Console screenshots và không chứa credentials, full ARN, account-specific bucket names hoặc inactive API URLs.

---

[Trước](/vi/1-worklog/1.7-week7/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.9-week9/)
