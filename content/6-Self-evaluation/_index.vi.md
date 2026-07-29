---
title: "Tự đánh giá"
date: 2024-01-01
weight: 6
chapter: false
pre: " <b> 6. </b> "
---

## Tự đánh giá

Trong thời gian thực tập tại **Amazon Web Services Viet Nam Company Limited** trong chương trình **Workforce Bootcamp - First Cloud AI Journey** từ **01/06/2026 đến 23/08/2026**, tôi có cơ hội tìm hiểu các dịch vụ AWS, tham gia các sự kiện kỹ thuật và xây dựng project cá nhân tên là **Xây dựng và triển khai hệ thống đánh giá chất lượng và rủi ro cho AI Coding Agent trên AWS SageMaker**.

Kỳ thực tập giúp tôi kết nối kiến thức đã học ở trường với một workflow cloud và AI thực tế hơn. SageMaker Training quota không khả dụng tại `ap-southeast-1`, nên một local artifact trước đó hỗ trợ historical serving tại Region này. Sau khi quota cho `1 x ml.m5.large` được duyệt tại `us-east-1`, tôi hoàn tất managed Training, held-out Evaluation, Experiments/HPO, Pipeline, Model Registry và Model Monitor acceptance, đồng thời giữ evidence cho Processing, Endpoint, Lambda, API Gateway, Data Capture, IAM và CloudWatch.

Về thái độ làm việc, tôi tự triển khai cá nhân nhưng vẫn học hỏi qua thảo luận nhóm, tài liệu AWS, workshop và các sự kiện cộng đồng. Tôi tập trung hoàn thành governed workflow, thu thập durable evidence, cleanup paid resources, xây dựng workshop website song ngữ và trình bày limitation minh bạch.

## Bảng tiêu chí tự đánh giá

| STT | Tiêu chí | Mô tả | Tốt | Khá | Trung bình |
|---|---|---|---|---|---|
| 1 | **Kiến thức và kỹ năng chuyên môn** | Hiểu AWS services, ML workflow, API integration và áp dụng kiến thức kỹ thuật vào project | ✅ | ☐ | ☐ |
| 2 | **Khả năng học hỏi** | Có khả năng học AWS services mới, đọc documentation, thử command và rút kinh nghiệm từ lỗi | ✅ | ☐ | ☐ |
| 3 | **Tính chủ động** | Chủ động chọn topic thực tế, xác định MVP scope và thu thập evidence | ✅ | ☐ | ☐ |
| 4 | **Tinh thần trách nhiệm** | Hoàn thành các phần project, ghi rõ limitation và chú ý cleanup để kiểm soát chi phí | ✅ | ☐ | ☐ |
| 5 | **Kỷ luật** | Bám theo timeline thực tập, duy trì worklog theo tuần và tổ chức deliverables | ☐ | ✅ | ☐ |
| 6 | **Tinh thần cầu tiến** | Sẵn sàng tiếp nhận feedback, sửa nội dung, cải thiện diagrams và cập nhật cấu trúc báo cáo | ✅ | ☐ | ☐ |
| 7 | **Giao tiếp** | Trình bày mục tiêu project, AWS architecture, limitations và kết quả bằng tiếng Anh và tiếng Việt | ☐ | ✅ | ☐ |
| 8 | **Làm việc nhóm** | Tham gia học tập theo nhóm, thảo luận và sự kiện cộng đồng trong khi vẫn hoàn thành phần cá nhân | ☐ | ✅ | ☐ |
| 9 | **Tác phong chuyên nghiệp** | Tôn trọng môi trường học tập, quy định tham gia sự kiện, yêu cầu báo cáo và sử dụng AWS có trách nhiệm | ✅ | ☐ | ☐ |
| 10 | **Kỹ năng giải quyết vấn đề** | Xử lý blocker như quota limitation, API integration issues và thu thập evidence sau cleanup | ✅ | ☐ | ☐ |
| 11 | **Đóng góp cho project/team** | Xây dựng MVP workflow hoàn chỉnh và chuẩn bị report theo dạng workshop có thể tái sử dụng | ✅ | ☐ | ☐ |
| 12 | **Đánh giá tổng thể** | Đánh giá chung về thái độ học tập, kết quả project và chất lượng documentation trong kỳ thực tập | ✅ | ☐ | ☐ |

## Điểm mạnh

- **Khả năng tự học:** Tôi có thể tự tìm hiểu AWS services thông qua documentation, tutorials, CLI checks, screenshots và triển khai thực tế.
- **Triển khai kỹ thuật:** Tôi hoàn tất managed AWS ML/MLOps path từ trajectories và Processing đến Training, held-out Evaluation, Experiments/HPO, Pipeline, conditional Registry registration, historical serving và monitoring acceptance.
- **Governance:** Gate `risky_recall >= 0.85` chỉ cho phép registration; Registry versions `/1` và `/2` vẫn `PendingManualApproval`, còn human review cùng deterministic safety rules giữ quyền quyết định.
- **Observability và cost control:** Tôi nghiệm thu Data Capture, Model Monitor, CloudWatch metrics, một dashboard và bảy actions-disabled alarms, sau đó xác minh cleanup short-lived paid resources.
- **Đánh giá trung thực:** Tôi test frozen model local trên 40 external trajectories và báo cáo macro F1 giảm từ synthetic `1.00` xuống external `0.1212`, thay vì xem perfect held-out scores là production quality.
- **Documentation dựa trên bằng chứng:** Tôi giữ screenshots, source evidence, S3 artifacts, reports, API responses, monitoring records và cleanup notes mà không công bố raw external trajectories.

## Điểm cần cải thiện

- **Sự tự tin khi giao tiếp:** Tôi cần tiếp tục cải thiện cách trình bày technical trade-offs bằng lời nói, đặc biệt khi giải thích AWS architecture và limitations cho người khác.
- **Quản lý thời gian:** Tôi cần lên kế hoạch documentation và screenshot collection sớm hơn, tránh để quá nhiều phần chỉnh sửa báo cáo vào cuối kỳ.
- **Dữ liệu evaluation:** External pilot chỉ có 40 mẫu, chỉ hai mẫu mang nhãn risky và AI-assisted labels có full-axis agreement `7.5%`; tôi cần dataset đại diện lớn hơn với independent human annotation.
- **Độ tin cậy của parser và runtime:** Tôi cần review missing-field/default-value behavior và pin runtime tương thích thay vì phụ thuộc LabelEncoder được tạo bằng scikit-learn `0.24.1` nhưng load dưới `1.8.0`.
- **Đánh giá model:** Chỉ nên cân nhắc calibration hoặc cost-sensitive learning sau khi có human-labeled data tốt hơn, tiếp theo là governed evaluation và reviewed release sau manual Registry approval.
- **Viết kỹ thuật bằng tiếng Anh:** Tôi cần tiếp tục cải thiện technical writing để báo cáo ngắn gọn, chính xác và tự nhiên hơn ở cả tiếng Anh và tiếng Việt.

## Nhận xét tổng quan

Kỳ thực tập giúp tôi hiểu rằng một cloud AI project không chỉ là train model. Một workflow hoàn chỉnh còn bao gồm thiết kế dữ liệu, cấu trúc lưu trữ, IAM permissions, processing jobs, model packaging, endpoint deployment, API integration, logs, cost control, cleanup và documentation.

Bài học giá trị nhất là managed execution thành công không đồng nghĩa model có khả năng generalize. AWS workflow, governance, serving và monitoring evidence đã hoàn tất, nhưng External/OOD pilot local cho thấy khoảng cách lớn giữa synthetic và public trajectories. Báo cáo cả hai kết quả—đồng thời giữ rõ human review, hard rules, manual approval và cleanup boundaries—giúp tôi hình thành tư duy kỹ sư chuyên nghiệp hơn.
