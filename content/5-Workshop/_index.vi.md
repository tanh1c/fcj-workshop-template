---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

## AI Coding Agent Risk Scoring trên AWS

Workshop này trình bày luồng dữ liệu, managed ML, governance, historical serving và monitoring đã hoàn thiện để chấm điểm trajectory của AI Coding Agent. Nội dung dùng evidence đã nghiệm thu và command với placeholder an toàn; không cần chạy lại tài nguyên trả phí chỉ để theo workshop.

## Các phần trong workshop

1. [Tổng quan workshop](/vi/5-workshop/5.1-workshop-overview/)
2. [Chuẩn bị và safety gate](/vi/5-workshop/5.2-prerequiste/)
3. [Kiến trúc split-Region](/vi/5-workshop/5.3-s3-vpc/)
4. [Chuẩn bị trajectory dataset](/vi/5-workshop/5.4-s3-onprem/)
5. [Chạy SageMaker Processing](/vi/5-workshop/5.5-policy/)
6. [Managed Training, Evaluation và HPO](/vi/5-workshop/5.6-cleanup/)
7. [Pipeline và Model Registry governance](/vi/5-workshop/5.7-deploy-endpoint/)
8. [Historical Endpoint và scoring API](/vi/5-workshop/5.8-scoring-api/)
9. [Kiểm tra end-to-end và evidence](/vi/5-workshop/5.9-end-to-end-demo/)
10. [Monitoring và kiểm soát chi phí](/vi/5-workshop/5.10-monitoring-cost-control/)
11. [Cleanup](/vi/5-workshop/5.11-cleanup/)

Managed Registry packages tại `us-east-1` và artifact train local trước đó dùng cho historical serving tại `ap-southeast-1` là hai evidence track riêng. Vượt model-quality gate chỉ cho phép registration, không tự approve hoặc deploy model.
