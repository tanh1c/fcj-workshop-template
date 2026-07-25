---
title: "Kiến trúc split-Region"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 5.3. </b> "
---

Project hoàn thiện tách managed ML governance khỏi historical serving để evidence không tạo hiểu nhầm về một deployment chưa thực hiện.

![Kiến trúc AWS split-Region hoàn thiện cho AI Coding Agent Risk Scoring](/images/2-Proposal/ai-agent-risk-architecture.webp)

*Figure 1. Managed ML và governance chạy tại `us-east-1`; historical serving ngắn hạn cùng API evidence chạy tại `ap-southeast-1`.*

Việc split Region là cần thiết vì quota SageMaker Training mà project yêu cầu không được cấp tại `ap-southeast-1`. Quota cho `1 x ml.m5.large` được duyệt tại `us-east-1`, cho phép managed Training cùng evaluation, HPO, Pipeline, Registry và Model Monitor phụ thuộc chạy tại đây trong khi evidence trước đó ở Singapore vẫn được giữ tách biệt.

## Trách nhiệm theo Region

| Region | Workload đã nghiệm thu |
|---|---|
| `us-east-1` | Managed Training, held-out evaluation artifacts, Experiments/HPO, Pipeline, Model Registry và Model Monitor acceptance. |
| `ap-southeast-1` | Processing/Studio lịch sử, Endpoint ngắn hạn, Lambda, API Gateway, Data Capture và CloudWatch serving evidence. |

## Ranh giới evidence quan trọng

Pipeline đăng ký model đạt gate với trạng thái `PendingManualApproval`; Pipeline không approve hoặc deploy model. Registry versions `agent-risk-scorer/1` và `/2` không được dùng bởi historical Endpoint. Endpoint đó dùng XGBoost artifact train local trước đó và đã bị xóa sau acceptance.

Tiếp tục với:

- [Luồng dữ liệu và managed ML](/vi/5-workshop/5.3-s3-vpc/5.3.1-create-gwe/)
- [Luồng governance, inference và monitoring](/vi/5-workshop/5.3-s3-vpc/5.3.2-test-gwe/)
