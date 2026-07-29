---
title: "Chuẩn bị và safety gate"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 5.2. </b> "
---

## Yêu cầu local

- Python 3.12 hoặc phiên bản Python 3 tương thích.
- AWS CLI đã cấu hình cho tài khoản AWS được phép sử dụng.
- Source code project và dependencies trong `requirements.txt`.

```bash
pip install -r requirements.txt
PYTHONPATH=demo_repo pytest demo_repo/tests -v
pytest agent data_generation preprocessing training inference pipelines lambda monitoring -q
```

## Ranh giới AWS

| Phạm vi | Region |
|---|---|
| Managed Training, evaluation, HPO, Pipeline, Registry, Model Monitor evidence | `us-east-1` |
| Historical Processing và optional serving/API ngắn hạn | `ap-southeast-1` |

Ranh giới này xuất phát từ quota: quota SageMaker Training cần thiết không được cấp tại `ap-southeast-1`, trong khi quota cho `1 x ml.m5.large` được duyệt tại `us-east-1`.

Dùng placeholder trong mọi command có thể tái sử dụng:

```bash
TRAINING_BUCKET="<us-east-1-training-bucket>"
SERVING_BUCKET="<ap-southeast-1-serving-bucket>"
SAGEMAKER_ROLE_ARN="<sagemaker-execution-role-arn>"
LAMBDA_ROLE_ARN="<lambda-execution-role-arn>"
```

Giữ S3 Block Public Access, tách SageMaker role và Lambda role, cấp least privilege, không lưu hoặc hiển thị credentials.

## Trạng thái Studio đã nghiệm thu

![SageMaker Studio Domain, User Profile và Space đang hoạt động](/images/5-Workshop/current/sagemaker-studio-domain-user-space-inservice.png)

*Hình 1. Studio Domain, User Profile và Space đều ở trạng thái `InService` tại `ap-southeast-1`.*

![SageMaker Studio không có ứng dụng đang chạy](/images/5-Workshop/current/sagemaker-studio-zero-running-apps.png)

*Hình 2. Lần kiểm tra nghiệm thu cho thấy không có Studio application đang chạy, vì vậy không còn notebook compute hoạt động.*

## Confirmation gate cho thao tác trả phí

Đọc workshop và xem retained evidence không tạo AWS resource. Trước mọi command có phí, phải xác nhận rõ:

```text
Region và account: đã kiểm tra
Resources: liệt kê chính xác job/endpoint/function/API
Instance type và số job tối đa: đã kiểm tra
Traffic: giới hạn trong số request đã nêu
Cleanup owner và command: đã sẵn sàng
Credentials hoặc login details trên màn hình: không có
```

Không chạy lại Processing, Training, HPO, Pipeline, Model Monitor hoặc Endpoint chỉ để tạo lại screenshot. Nếu live serving đã được cho phép, chỉ tạo một Endpoint, Lambda và HTTP API ngắn hạn tại `ap-southeast-1`, gửi số request đã định rồi cleanup ngay.
