---
title: "Tuần 5: SageMaker Processing và feature engineering"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 1.5. </b> "
---

## 29/06/2026 - 05/07/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

## Mục tiêu

Triển khai bước managed processing để chuyển raw trajectory JSONL data thành tabular ML features.

## Bối cảnh

Đây là bước AWS ML workflow lớn đầu tiên. Mục tiêu là chứng minh raw logs có thể được xử lý trên SageMaker infrastructure và ghi lại về S3 dưới dạng train/validation/test CSV sạch.

## AWS services và kiến thức đã tìm hiểu

Tuần này tập trung vào Amazon SageMaker Processing, AWS managed ML workflow service đầu tiên được dùng trong project. Tôi tìm hiểu cả mục đích của service lẫn các thao tác cụ thể để chạy một processing job.

- **Purpose of SageMaker Processing:** Học rằng Processing được dùng để chạy data preparation, validation, transformation và feature engineering jobs trên managed infrastructure.
- **Processing script:** Tìm hiểu cách Python script nhận input files, transform records và ghi output files bên trong processing container.
- **ProcessingInput:** Rà soát cách input data từ S3 được mount vào processing job để script đọc raw JSONL files.
- **ProcessingOutput:** Rà soát cách generated files được upload từ container trở lại S3 output prefix khi job kết thúc.
- **Processing image và instance type:** Tìm hiểu rằng processing job cần container image và compute instance type, ví dụ CPU-based instance cho tabular preprocessing.
- **IAM execution role:** Kiểm tra SageMaker cần quyền đọc raw S3 prefix và ghi processed S3 prefix.
- **CloudWatch logs:** Rà soát logs như cách chính để debug processing script có chạy thành công hay không.
- **Project application:** Áp dụng các khái niệm này để chuyển trajectory JSONL logs thành `train.csv`, `validation.csv` và `test.csv`.

Phần này cho thấy processing step không chỉ là code transformation, mà còn là một AWS managed job có S3 input/output, IAM, compute và logs.

## Bảng công việc theo ngày

| Ngày | Công việc đã thực hiện |
|---|---|
| 29/06/2026 | Triển khai logic extract feature từ trajectory JSONL records sang tabular fields. |
| 30/06/2026 | Test local processing output và verify việc tạo train, validation, test CSV. |
| 01/07/2026 | Chuẩn bị SageMaker Processing configuration với S3 input/output paths. |
| 02/07/2026 | Chạy hoặc verify SageMaker Processing workflow và xác nhận processed output locations. |
| 03/07/2026 | Kiểm tra processed CSV trên S3 và evidence CLI/processing cho completed job. |
| 04/07/2026 - 05/07/2026 | Chụp S3 processed output screenshots và ghi lại feature engineering trong workshop. |


## Công việc kỹ thuật

- Triển khai feature engineering cho file counts, modified file counts, command counts, test/lint pass indicators, sensitive file flags, risky command flags, network command flags và diff-size features.
- Chuyển labels thành target values sẵn sàng cho model, đồng thời giữ ý nghĩa ban đầu để giải thích trong báo cáo.
- Cấu hình SageMaker Processing input/output paths để processing job đọc raw JSONL từ S3 và ghi CSV outputs về S3.
- Kiểm tra job status và logs qua SageMaker/CloudWatch để xác nhận processing job hoàn tất và giải phóng compute resources.

## Deliverables

- **Hoàn thành feature engineering script.**
- **Tạo train/validation/test CSV outputs.**
- **Verify SageMaker Processing job.**
- **Ghi lại processing step để tái hiện trong workshop.**

## Khó khăn và cách xử lý

**Khó khăn:** Vấn đề chính là giữ output processing đủ đơn giản cho XGBoost nhưng vẫn giữ các safety signals quan trọng.

**Cách xử lý:** Features được giữ ở dạng tabular và dễ giải thích để workshop có thể trình bày vì sao một risky run nhận risk score cao hơn.

## Liên hệ với project chính

Tuần này đóng góp vào MVP cuối bằng cách củng cố luồng từ **bằng chứng hành vi của AI coding agent** đến **workflow đánh giá rủi ro trên AWS**. Nội dung giúp workshop cuối không chỉ là giải thích khái niệm, mà còn bám theo đúng trình tự triển khai thực tế của project.

## Ảnh bằng chứng

![Processed CSV outputs trên S3](/images/worklog/week05-processed-csv-s3.png)

![Bằng chứng SageMaker Processing job từ CLI](/images/worklog/week05-sagemaker-processing-job-cli.png)

Các ảnh chụp cho thấy bước processing đã tạo các file train, validation và test CSV trong S3 prefix của project.

## Bằng chứng và tài liệu tham khảo đã tìm hiểu

- [SageMaker Processing](https://docs.aws.amazon.com/sagemaker/latest/dg/processing-job.html)
- [SageMaker Python SDK Processing API](https://sagemaker.readthedocs.io/en/stable/amazon_sagemaker_processing.html)
- [CloudWatch Logs](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html)

---

[Trước](/vi/1-worklog/1.4-week4/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.6-week6/)
