---
title: "Chia sẻ và phản hồi"
date: 2024-01-01
weight: 7
chapter: false
pre: " <b> 7. </b> "
---

## Chia sẻ và phản hồi

Phần này trình bày cảm nhận và phản hồi cá nhân của tôi sau khi tham gia chương trình **Workforce Bootcamp - First Cloud AI Journey** tại **Amazon Web Services Viet Nam Company Limited**. Nội dung được viết dựa trên quá trình học AWS fundamentals, tham gia các sự kiện cộng đồng và xây dựng project **Xây dựng và triển khai hệ thống đánh giá chất lượng và rủi ro cho AI Coding Agent trên AWS SageMaker**.

## Đánh giá tổng quan

**1. Môi trường học tập**  
Chương trình tạo ra một môi trường học tập mở và có tính tự định hướng cao. Tôi có không gian để tự tìm hiểu AWS services, thử command, đọc documentation và xây dựng project từ ý tưởng đến MVP. Cách học này hữu ích vì giúp tôi chủ động hơn thay vì chỉ làm theo hướng dẫn cố định.

**2. Sự hỗ trợ từ chương trình và cộng đồng**  
Dù không có mentor cố định cho project cá nhân, tôi vẫn học được nhiều thông qua thảo luận nhóm, tài liệu AWS, workshop và các sự kiện cộng đồng của FCAJ. Các sự kiện mang lại góc nhìn kỹ thuật và định hướng nghề nghiệp hữu ích, đặc biệt về cloud engineering, DevOps, security, data analytics và các use case generative AI.

**3. Mức độ liên quan với chuyên ngành**  
Kỳ thực tập phù hợp với nền tảng Computer Science của tôi vì kết hợp lập trình, xử lý dữ liệu, machine learning, API design và triển khai hệ thống. Kiến thức ở trường giúp tôi hiểu phần model và data workflow, còn chương trình giúp tôi áp dụng kiến thức đó vào môi trường cloud với các dịch vụ AWS.

**4. Cơ hội học tập và phát triển kỹ năng**  
Tôi cải thiện cả kỹ năng kỹ thuật và kỹ năng chuyên nghiệp trong chương trình. Về kỹ thuật, tôi thực hành Amazon S3, SageMaker Processing, managed XGBoost Training và Evaluation, Experiments/HPO, Pipeline, Model Registry, historical Endpoint serving, Lambda, API Gateway, Data Capture, Model Monitor, IAM và CloudWatch. Về chuyên nghiệp, tôi luyện tập governance design, cost-aware cleanup, evidence collection, honest evaluation và viết workshop song ngữ.

**5. Văn hóa chương trình và tinh thần cộng đồng**  
Chương trình khuyến khích học thông qua chia sẻ và tham gia cộng đồng. Các sự kiện kỹ thuật giúp kỳ thực tập không chỉ xoay quanh project cá nhân, vì tôi có cơ hội lắng nghe nhiều diễn giả và hiểu cách kỹ năng AWS được áp dụng trong công ty, định hướng nghề nghiệp và hệ thống thực tế.

**6. Chính sách thực tập và phạm vi project**  
Hình thức tự học phù hợp với việc khám phá một chủ đề kỹ thuật cá nhân. Training quota không khả dụng tại `ap-southeast-1` ban đầu yêu cầu một historical-serving path có giới hạn; quota được duyệt tại `us-east-1` sau đó cho phép hoàn tất managed ML và governance workflow. Điều này giúp tôi học cách điều chỉnh Region cùng evidence strategy mà không nhầm temporary workaround với kiến trúc cuối.

## Điều tôi hài lòng nhất

Điều tôi hài lòng nhất trong kỳ thực tập là hoàn thành governed end-to-end workflow thay vì chỉ dừng ở proposal. Bắt đầu từ AI coding-agent trajectories, tôi hoàn tất managed Processing, Training, held-out Evaluation, Experiments/HPO, conditional Registry registration, historical serving, Data Capture, Model Monitor, CloudWatch acceptance và xác minh cleanup paid resources.

Một giá trị quan trọng khác là kiểm tra perfect synthetic result thay vì bảo vệ nó. Macro F1 của frozen model giảm từ `1.00` nội bộ xuống `0.1212` trên External/OOD pilot 40 mẫu. Kết quả này làm report mạnh hơn vì cho thấy manual approval, human review, hard safety rules và representative human-labeled data vẫn cần thiết.

## Gợi ý cải thiện

- Cung cấp checklist sớm về AWS student-account quota, đặc biệt cho SageMaker Training và các tài nguyên liên quan đến endpoint.
- Cung cấp checklist kiểm soát chi phí trước khi learner deploy các tài nguyên có thể phát sinh chi phí liên tục, ví dụ SageMaker Endpoint.
- Chia sẻ thêm các architecture mẫu cho project ML/MLOps phù hợp với người mới, sử dụng S3, SageMaker, Lambda, API Gateway, IAM và CloudWatch.
- Bổ sung các milestone review tùy chọn để người tham gia nhận feedback trước giai đoạn hoàn thiện báo cáo cuối.
- Hướng dẫn rõ hơn về các loại evidence nên chụp lại trước khi cleanup AWS resources.

## Khả năng giới thiệu chương trình

Tôi sẽ giới thiệu chương trình First Cloud AI Journey cho bạn bè muốn học cloud computing thông qua thực hành. Chương trình đặc biệt hữu ích cho sinh viên đã có nền tảng lập trình hoặc machine learning cơ bản và muốn hiểu cách các kỹ năng đó được đưa vào một workflow AWS thực tế.

Chương trình đòi hỏi tính kỷ luật tự học vì nhiều phần được tự quản lý, nhưng đây cũng là một điểm mạnh. Cách học này giúp người tham gia rèn tính độc lập, thói quen documentation và tư duy kỹ sư thực tế hơn.

## Kỳ vọng trong tương lai

Nếu tiếp tục phát triển project, tôi sẽ bắt đầu bằng representative trajectories có independent human labels và thêm adapters cho nhiều agent frameworks. Chỉ sau đó tôi mới đánh giá calibrated thresholds hoặc cost-sensitive learning, pin runtimes tương thích, đồng thời thiết kế canary/rollback controls và một reviewed deployment pipeline riêng cho model đã nhận manual Registry approval.
