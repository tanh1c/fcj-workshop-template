---
title: "Event 1"
date: 2024-01-01
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

## Event 1: FiRST CLOUD JOURNEY Community Day

| Mục | Thông tin |
|---|---|
| Tên sự kiện | FiRST CLOUD JOURNEY Community Day |
| Ngày và giờ | 09:00, 06/06/2026 |
| Địa điểm | 26th Floor, Bitexco Tower, 02 Hai Trieu Street, Saigon Ward, Ho Chi Minh City |
| Vai trò | Attendee |
| Mục đích đăng ký | Attend Events |
| Ca làm việc | Fulltime |
| Người tham gia | Chu Nguyễn Tuấn Anh |
| Trường đại học | Trường Đại học Bách khoa |
| Mã số sinh viên | 2352022 |

## Tổng quan

**FiRST CLOUD JOURNEY Community Day** là một sự kiện học tập cộng đồng với nhiều phần chia sẻ đến từ các mảng system administration, cloud-native development, cybersecurity, game development, teamwork và artificial intelligence. Giá trị của sự kiện nằm ở việc nội dung không chỉ tập trung vào một công nghệ đơn lẻ, mà cho thấy cách cloud, DevOps, security, GenAI, teamwork và kinh nghiệm kỹ thuật thực tế liên kết với nhau trong các project công nghệ hiện đại.

Với vai trò attendee, tôi tham gia sự kiện để học hỏi từ các câu chuyện thực tế và những phiên chia sẻ kỹ thuật. Các nội dung trong sự kiện giúp tôi mở rộng góc nhìn vượt ra ngoài phạm vi project thực tập, đồng thời hiểu rõ hơn cách các ý tưởng AWS và cloud-native có thể được áp dụng trong những bối cảnh gần với thực tế production.

## Diễn giả và chủ đề

Sự kiện có sự tham gia của nhiều diễn giả với nền tảng khác nhau:

| Diễn giả | Chủ đề |
|---|---|
| Trần Trung Vinh | Hành trình nghề nghiệp từ IT Helpdesk đến Senior Sysadmin, Cloud và DevOps |
| Trương Huy Phước | Tư duy quản lý dự án và làm việc nhóm hiệu quả |
| Bảo Huỳnh | Docker và đóng gói ứng dụng theo hướng cloud-native |
| Lê Hoàng Gia Đại | AWS WAF và hệ thống phát hiện xâm nhập mạng dựa trên Machine Learning |
| Nguyễn Quốc Bảo | Phát triển game multiplayer với AWS WebSockets và Godot 4 |
| Việt Phát | GraphRAG với Generative AI, Amazon Bedrock và Amazon Neptune |

## Nội dung chính và kiến thức thu được

### Hành trình từ IT Helpdesk đến Cloud và DevOps

Một trong những phần chia sẻ thực tế nhất là hành trình phát triển từ vị trí IT Helpdesk lên Senior Sysadmin, Cloud và DevOps. Bài học chính là sự nghiệp công nghệ có thể được xây dựng bằng quá trình tự học bền bỉ, thực hành qua lab và tích lũy kinh nghiệm vận hành thực tế.

Phiên chia sẻ nhấn mạnh các thói quen quan trọng như học sâu Linux và networking, tài liệu hóa cấu hình và runbook, xây dựng monitoring trước khi sự cố xảy ra, và không kiểm thử trực tiếp trên production. Thông điệp này rất phù hợp với project thực tập của tôi vì project cũng cần validation cẩn thận, thu thập bằng chứng và vận hành AWS có kiểm soát chi phí.

### Làm việc nhóm và triển khai project hiệu quả

Một phiên khác tập trung vào hiệu suất làm việc nhóm. Diễn giả giải thích rằng năng suất cá nhân khác với năng suất của cả nhóm, và một đội ngũ tốt cần mục tiêu chung rõ ràng, phân công phù hợp, giao tiếp cởi mở, lắng nghe chủ động và trách nhiệm cá nhân.

Bốn quy tắc teamwork nổi bật gồm:

- Mục tiêu chung rõ ràng.
- Đúng người, đúng việc.
- Giao tiếp cởi mở và lắng nghe chủ động.
- Trách nhiệm cá nhân với phần việc được giao.

Nội dung này giúp tôi nhìn nhận rằng internship work không chỉ là hoàn thành task kỹ thuật, mà còn là biết báo cáo tiến độ, thống nhất kỳ vọng và sử dụng các công cụ như Trello, ClickUp, Google Workspace, Slack hoặc Discord để phối hợp hiệu quả hơn.

### Docker và Containerization

Phiên Docker giải thích vì sao containerization đóng vai trò quan trọng trong quy trình phát triển phần mềm hiện đại. So với virtual machine truyền thống, container nhẹ hơn, khởi động nhanh hơn và dễ di chuyển giữa các môi trường. Docker đóng gói ứng dụng cùng dependency và cấu hình, giúp giảm vấn đề phổ biến “chạy được trên máy tôi nhưng lỗi trên môi trường khác”.

Phiên chia sẻ cũng giới thiệu các khái niệm cốt lõi như Dockerfile, image layers, build cache, container lifecycle commands, network và volume. Điều này giúp tôi hiểu rõ hơn vì sao container được dùng rộng rãi trong CI/CD pipeline, microservices, cloud-native applications và modernization cho hệ thống cũ.

### AWS WAF và ML-based Network Intrusion Detection

Phiên cybersecurity là một trong những phần kỹ thuật đáng chú ý nhất của sự kiện. Nội dung giải thích rằng AWS WAF có thể chặn các mối đe dọa phổ biến ở Layer 7 như SQL Injection, XSS và bot traffic, nhưng hệ thống rule-based truyền thống có thể chưa đủ để phát hiện hành vi mới hoặc zero-day.

Giải pháp được trình bày là kết hợp AWS WAF với hệ thống Network Intrusion Detection dựa trên Machine Learning. Diễn giả trình bày workflow sử dụng dataset traffic mạng, làm sạch dữ liệu, cân bằng lớp và so sánh nhiều mô hình như Random Forest, LightGBM, MLP, 1D-CNN và XGBoost. Kiến trúc có liên quan đến các AWS services như WAF, ALB, EC2, VPC, Kinesis Data Firehose, S3, Lambda, SNS, Security Hub và GuardDuty.

Phiên này liên hệ mạnh với project của tôi vì cả hai hệ thống đều dùng ML để phát hiện hành vi rủi ro. Trong project của tôi, nguồn rủi ro là trajectory của AI coding agent; trong phiên sự kiện, nguồn rủi ro là network traffic. Bài học chung là ML-based detection nên bổ sung cho rule-based protection, không nên thay thế hoàn toàn các quy tắc an toàn cứng.

### Multiplayer Game Architecture với AWS WebSockets và Godot 4

Phiên game development giới thiệu cách xây dựng tính năng multiplayer realtime bằng Godot 4 và kiến trúc WebSocket trên AWS. Diễn giả so sánh UDP/ENet, HTTP polling và WebSocket, sau đó giải thích vì sao WebSocket phù hợp với lobby system, chat và turn-based game.

Kiến trúc serverless được trình bày sử dụng API Gateway WebSocket routes, AWS Lambda cho game logic phi trạng thái và DynamoDB để lưu trạng thái kết nối người chơi cũng như tiến trình game. Các vấn đề thực tế như stale connections, `GoneException`, DynamoDB scan cost và bản chất stateless của Lambda cũng được phân tích.

Giá trị lớn nhất của phiên này là hiểu về trade-off trong kiến trúc. Serverless WebSocket giúp giảm gánh nặng vận hành và chi phí cho một số loại game, nhưng yêu cầu quản lý trạng thái cẩn thận và không phù hợp với game realtime quy mô lớn có tính toán vật lý phức tạp. Với các trường hợp đó, AWS GameLift sẽ phù hợp hơn.

### GraphRAG với Amazon Bedrock và Amazon Neptune

Phiên AI giới thiệu GraphRAG, một hướng cải thiện Retrieval-Augmented Generation bằng cách kết hợp Generative AI với knowledge graph. RAG truyền thống thường truy xuất các đoạn text rời rạc, chưa đủ tốt cho những câu hỏi cần suy luận qua nhiều thực thể hoặc nhiều tài liệu khác nhau.

GraphRAG lưu thực thể dưới dạng node và quan hệ dưới dạng edge, cho phép hệ thống duyệt đồ thị để lấy bối cảnh đầy đủ hơn. Phiên chia sẻ đề cập hai hướng triển khai trên AWS: hướng managed với Amazon Bedrock Knowledge Bases và Amazon Neptune Analytics, hoặc hướng custom bằng các framework như LlamaIndex kết hợp Amazon Neptune.

Nội dung này giúp tôi hiểu rõ hơn rằng chất lượng hệ thống GenAI không chỉ phụ thuộc vào mô hình ngôn ngữ lớn, mà còn phụ thuộc rất nhiều vào cách tổ chức dữ liệu, retrieval và biểu diễn quan hệ tri thức.

## Trải nghiệm tại sự kiện

Sự kiện để lại ấn tượng tốt vì kết hợp được chiều sâu kỹ thuật với các câu chuyện nghề nghiệp thực tế. Tôi được nghe chia sẻ từ các diễn giả có cả background industry và university, quan sát các demo thực tế và học những vấn đề thường không xuất hiện trong tutorial cơ bản, ví dụ stale WebSocket connections, database scan cost, Docker layer caching và đánh giá mô hình ML trong bài toán security.

Việc tham gia trực tiếp cũng giúp tôi rèn luyện tác phong chuyên nghiệp: đến đúng địa điểm, theo dõi agenda, lắng nghe chủ động, ghi chú và liên hệ kiến thức học được với project cá nhân. Sự kiện không chỉ mang lại kiến thức mà còn tạo thêm động lực và cảm giác gắn kết với cộng đồng cloud và AI.

## Bài học rút ra

- Thực hành có giá trị thuyết phục hơn lý thuyết đơn thuần. Việc tự xây lab, ghi chép quá trình làm việc và có portfolio chạy được là rất quan trọng cho phát triển nghề nghiệp.
- Mọi quyết định kiến trúc đều là trade-off. Container, virtual machine, serverless WebSocket, dedicated game server, rule-based security và ML-based detection đều phù hợp với những bối cảnh khác nhau.
- Security nên kết hợp prevention và detection. Các hệ thống rule-based như WAF hữu ích, nhưng behavior-based ML detection có thể bổ sung thêm một lớp bảo vệ.
- Cloud-native và AI đang hội tụ mạnh. Các managed services như Lambda, API Gateway, DynamoDB, Bedrock và Neptune giúp developer xây dựng hệ thống nâng cao nhanh hơn.
- Teamwork cần có cấu trúc. Mục tiêu rõ ràng, giao tiếp tốt và trách nhiệm cá nhân quan trọng không kém năng lực kỹ thuật.

## Liên hệ với project thực tập

Sự kiện này hữu ích trực tiếp với project thực tập **AI Coding Agent Risk Scoring on AWS SageMaker** của tôi. Phiên ML-based NIDS giúp tôi nhìn bài toán risk detection như sự kết hợp giữa model prediction và hard safety rules. Phiên Docker và DevOps củng cố tầm quan trọng của môi trường lặp lại được và quy trình deploy cẩn thận. Phiên teamwork giúp tôi cải thiện cách ghi worklog và trình bày giá trị project.

Quan trọng nhất, sự kiện củng cố quan điểm rằng các hệ thống AI-assisted cần được đánh giá bằng bằng chứng. Dù hệ thống đang phát hiện network attack, sinh câu trả lời bằng GraphRAG hay chấm điểm hành vi AI coding agent, quy trình kỹ thuật vẫn cần logs, metrics, validation và operational boundaries rõ ràng.

## Minh chứng đăng ký

| Chi tiết đăng ký | Giá trị |
|---|---|
| Họ và tên | Chu Nguyễn Tuấn Anh |
| Email | anh.chunguyentuan@hcmut.edu.vn |
| Số điện thoại | 0962037357 |
| Trường đại học | Trường Đại học Bách khoa |
| Mã số sinh viên | 2352022 |
| Ngày đăng ký | 06/06/2026 |
| Ca làm việc | Fulltime |
| Tầng | 26th Floor |
| Mục đích | Attend Events |

## Bằng chứng tham gia

Tôi đã quên chụp ảnh cá nhân trong sự kiện này. Vì vậy, evidence còn được lưu là lịch sử điểm danh trên FCAJ Portal bên dưới, thể hiện lượt check-in cho ca 09:00 ngày 06/06/2026 tại Tầng 26. Screenshot này chứng minh việc điểm danh trên portal, không phải ảnh cá nhân tại sự kiện.

![Evidence lịch sử điểm danh FCAJ Portal cho Event 1 ngày 06/06/2026](/images/events/event1-portal-checkin.png)

---

[Quay lại Events Participated](/vi/4-eventparticipated/) | [Tiếp](/vi/4-eventparticipated/4.2-event2/)
