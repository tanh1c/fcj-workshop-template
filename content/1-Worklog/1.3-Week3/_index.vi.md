---
title: "Tuần 3: Thiết kế dataset và mô phỏng trajectory logs"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 1.3. </b> "
---

## 15/06/2026 - 21/06/2026

**Hình thức làm việc:** Triển khai cá nhân kết hợp học tập và thảo luận theo nhóm.
**Chương trình:** Workforce Bootcamp - First Cloud AI Journey.
**Mentor:** Không có mentor cố định; công việc được tự quản lý, kết hợp tài liệu, tutorial và thảo luận với các bạn học.

## Mục tiêu

Thiết kế định dạng raw trajectory log và sinh sample đại diện để hỗ trợ feature engineering và supervised model training.

## Bối cảnh

Model không thể chấm điểm hành vi agent nếu không có bằng chứng có cấu trúc. Tuần này chuyển các khái niệm rủi ro trừu tượng thành field có thể lưu trữ, xử lý và chuyển thành ML features.

## AWS services và kiến thức đã tìm hiểu

Tuần này tập trung vào data foundation cần có trước khi dùng AWS ML services. Tôi tìm hiểu cách logs và labels nên được chuẩn bị để sau đó có thể xử lý bằng SageMaker và lưu trong S3.

- **JSONL log format:** Tìm hiểu vì sao JSONL phù hợp cho event-style logs: mỗi dòng là một record, dễ append, dễ inspect và phù hợp batch processing.
- **Raw data design for S3:** Rà soát nguyên tắc giữ raw files gần với dạng gốc để các bước processing sau có thể tái lập.
- **Schema consistency:** Tìm hiểu vì sao mỗi trajectory record cần có fields nhất quán như files read, files modified, commands run, test status, lint status và final summary.
- **Feature planning:** Xác định raw fields có thể trở thành tabular ML features: số file đọc, số file sửa, command count, diff size, tool count và latency.
- **Safety signal planning:** Tìm hiểu cách rule-based indicators trở thành model features, gồm sensitive-file access, destructive commands, network commands và unsupported success claims.
- **AWS connection:** Chuẩn bị dataset structure để về sau upload lên S3 và dùng làm input cho SageMaker Processing.

Tuần này chủ yếu nhằm chuẩn bị data đúng cách trước khi dùng managed services của AWS, vì raw schema kém sẽ làm processing và training sau này khó hơn.

## Bảng công việc theo ngày

| Ngày | Công việc đã thực hiện |
|---|---|
| 15/06/2026 | Chọn JSONL làm raw trajectory log format và xác định mỗi dòng đại diện cho một agent run. |
| 16/06/2026 | Thiết kế các field files_read, files_modified, commands_run, test/lint status, diff size và final_summary. |
| 17/06/2026 | Bổ sung safety signals như touched_sensitive_files, used_network_command và destructive_command_detected. |
| 18/06/2026 | Sinh local sample trajectories cho các scenario safe, failed, risky và require-review. |
| 19/06/2026 | Kiểm tra combined trajectory data và labels để xác nhận dataset hỗ trợ supervised learning. |
| 20/06/2026 - 21/06/2026 | Chụp dataset screenshots và link full JSONL evidence file để dễ đọc hơn. |


## Công việc kỹ thuật

- Chọn JSONL làm raw data format vì đơn giản, dễ append và phù hợp với cách lưu mỗi dòng là một agent run.
- Tạo các field như task description, files_read, files_modified, commands_run, tests_passed, lint_passed, diff_lines_added, diff_lines_deleted, touched_sensitive_files, used_network_command, destructive_command_detected và final_summary.
- Định nghĩa các label đại diện như safe, failed, risky và hallucinated_success để phản ánh cả vấn đề chất lượng lẫn an toàn.
- Sinh sample runs local gồm normal fixes, thiếu test evidence, truy cập file nhạy cảm, risky command attempts và diff lớn ngoài phạm vi.

## Deliverables

- **Định nghĩa raw JSONL schema.**
- **Sinh synthetic trajectory samples.**
- **Ghi lại labeling rules.**
- **Dataset sẵn sàng upload lên S3.**

## Khó khăn và cách xử lý

**Khó khăn:** Generated dataset cần đủ thực tế cho demo nhưng vẫn đủ đơn giản để giải thích trong workshop.

**Cách xử lý:** Schema tập trung vào các field có tín hiệu mạnh và dễ hiểu: phạm vi file, an toàn command, bằng chứng test/lint, truy cập nhạy cảm và diff size.

## Liên hệ với project chính

Tuần này đóng góp vào MVP cuối bằng cách củng cố luồng từ **bằng chứng hành vi của AI coding agent** đến **workflow đánh giá rủi ro trên AWS**. Nội dung giúp workshop cuối không chỉ là giải thích khái niệm, mà còn bám theo đúng trình tự triển khai thực tế của project.

## Ảnh và file bằng chứng

![Ảnh chụp trajectory JSONL sample](/images/worklog/week03-trajectory-jsonl.png)

![Folder sinh dataset](/images/worklog/week03-dataset-folder.png)

Ảnh JSONL sample có thể khó đọc khi hiển thị toàn trang, nên file raw trajectory đầy đủ được link trực tiếp tại đây: [week03-sample-trajectories.jsonl](/images/worklog/week03-sample-trajectories.jsonl).

## Bằng chứng và tài liệu tham khảo đã tìm hiểu

- [JSON Lines format](https://jsonlines.org/)
- [Amazon S3 User Guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/Welcome.html)
- [IAM security best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html)

---

[Trước](/vi/1-worklog/1.2-week2/) | [Quay lại Worklog](/vi/1-worklog/) | [Tiếp](/vi/1-worklog/1.4-week4/)
