---
title: "Bảo vệ ứng dụng Generative AI bằng Amazon Bedrock Guardrails"
date: 2026-07-18
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

## AWS Artificial Intelligence Blog | Bảo vệ ứng dụng Generative AI bằng Amazon Bedrock Guardrails

| Thông tin | Chi tiết |
|---|---|
| Ngày đăng | 18/07/2026 |
| Trạng thái | Đã đăng |
| Nền tảng | AWS Study Group - Facebook Group |
| Bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2214291746002452) |
| Bài AWS tham khảo | *Safeguard generative AI applications with Amazon Bedrock Guardrails* — AWS Artificial Intelligence Blog, 15/01/2026 |

Chào mọi người,

Mình đang tham gia chương trình FCAJ và trong quá trình tìm hiểu AWS, mình đã đọc được bài **“Safeguard generative AI applications with Amazon Bedrock Guardrails”**, được đăng trên AWS Artificial Intelligence Blog ngày 15/01/2026.

Bài viết trình bày cách xây dựng một Generative AI Gateway tập trung để kiểm soát prompt trước khi gửi đến Amazon Bedrock hoặc các mô hình AI bên ngoài.

## Bài toán đặt ra

Khi doanh nghiệp sử dụng nhiều ứng dụng và foundation model khác nhau, mỗi đội có thể tự xây dựng bộ lọc nội dung, quản lý API key và ghi log theo một cách riêng. Điều này dễ dẫn đến:

- Chính sách an toàn không đồng nhất.
- Dữ liệu cá nhân bị gửi đến model ngoài ý muốn.
- Credential nằm rải rác trong nhiều ứng dụng.
- Khó theo dõi chi phí và điều tra sự cố.
- Một số ứng dụng có lớp bảo vệ tốt, trong khi ứng dụng khác gần như không có.

Prompt engineering có thể yêu cầu model không trả lời nội dung nguy hiểm, nhưng đây không phải lớp bảo vệ đủ mạnh. Prompt vẫn có thể bị jailbreak hoặc prompt injection.

Giải pháp trong bài viết là đặt một gateway ở giữa ứng dụng và các LLM. Mọi request phải đi qua gateway trước khi được gửi tới model.

## Amazon Bedrock Guardrails làm gì?

Amazon Bedrock Guardrails cho phép cấu hình các chính sách như:

- Lọc nội dung nguy hiểm.
- Chặn các chủ đề không được phép.
- Phát hiện prompt attack.
- Chặn hoặc che thông tin nhận dạng cá nhân như email, số điện thoại và mã định danh.
- Lọc các từ hoặc cụm từ cụ thể.
- Kiểm tra mức độ liên quan và grounded của câu trả lời trong một số trường hợp.

Điểm đáng chú ý là API `ApplyGuardrail` có thể hoạt động độc lập với model inference.

Gateway có thể gửi prompt đến Guardrails để kiểm tra trước. Nếu prompt vi phạm chính sách, request sẽ bị chặn mà không cần gọi LLM. Nếu chứa thông tin nhạy cảm, dữ liệu có thể được che trước khi gửi tiếp.

![Luồng kiểm tra request bằng Amazon Bedrock Guardrails](/images/blogs/blog2-guardrails-request-flow.webp)

*Figure 1. Request validation flow using Amazon Bedrock Guardrails and the ApplyGuardrail API — Luồng kiểm tra request bằng Guardrails.*

## Luồng hoạt động của kiến trúc

```text
User hoặc Application
↓
Application Load Balancer
↓
Generative AI Gateway
Amazon ECS + AWS Fargate
↓
Amazon Bedrock Guardrails
↓
Block / Mask / Allow
↓
Amazon Bedrock hoặc External LLM
↓
Response trả về người dùng
```

Quy trình có thể được hiểu theo năm bước:

1. Người dùng hoặc ứng dụng gửi prompt đến gateway thông qua HTTPS.
2. Gateway gọi Amazon Bedrock Guardrails để kiểm tra nội dung đầu vào.
3. Guardrails quyết định chặn request, che dữ liệu nhạy cảm hoặc cho phép tiếp tục.
4. Gateway chuyển prompt đã được kiểm tra đến Amazon Bedrock hoặc một LLM bên ngoài.
5. Kết quả được trả về người dùng, đồng thời thông tin giao dịch được ghi lại để monitoring và phân tích.

![Kiến trúc Generative AI Gateway với Amazon Bedrock Guardrails](/images/blogs/blog2-generative-ai-gateway-architecture.webp)

*Figure 2. Generative AI Gateway reference architecture with Amazon Bedrock Guardrails — Kiến trúc tham khảo của gateway.*

Gateway trong bài được đóng gói thành container và chạy bằng Amazon Elastic Container Service trên AWS Fargate. Application Load Balancer phân phối request đến các container, còn AWS Secrets Manager lưu credential của các nhà cung cấp model bên ngoài.

## Logging và theo dõi hoạt động

Kiến trúc sử dụng hai nhánh quan sát chính.

Amazon CloudWatch thu thập log và metric để đội vận hành phát hiện lỗi, latency cao hoặc số lượng request bị Guardrails chặn tăng bất thường.

Dữ liệu giao dịch cũng có thể được đưa qua Amazon Kinesis Data Streams và Amazon Data Firehose, sau đó lưu vào Amazon S3. AWS Glue và Amazon Athena hỗ trợ truy vấn dữ liệu để phân tích mức sử dụng hoặc phân bổ chi phí cho từng đội và dự án.

Tuy nhiên, logging cũng tạo ra một rủi ro mới. Prompt và response có thể chứa dữ liệu nhạy cảm, vì vậy không nên mặc định lưu toàn bộ nội dung. Doanh nghiệp cần kiểm soát quyền truy cập, thời gian lưu trữ và những trường dữ liệu thực sự cần thiết.

## Một số tình huống ứng dụng

### Chặn tư vấn tài chính không phù hợp

Nếu chatbot không được phép đưa ra lời khuyên đầu tư, Guardrails có thể phát hiện chủ đề bị cấm và chặn prompt trước khi model được gọi.

Điều này giúp chính sách được thực thi tại gateway, thay vì phụ thuộc hoàn toàn vào việc model có tuân thủ system prompt hay không.

### Che thông tin cá nhân

Nếu prompt chứa email, số điện thoại hoặc mã định danh, Guardrails có thể thay thế dữ liệu đó bằng ký hiệu ẩn danh trước khi chuyển đến LLM.

Cách này giúp giảm lượng thông tin nhạy cảm được gửi tới model. Tuy nhiên, việc phát hiện PII vẫn có thể có false positive hoặc false negative, nên không thể coi đây là một hệ thống Data Loss Prevention hoàn chỉnh.

### Quản lý nhiều ứng dụng AI trong doanh nghiệp

Một doanh nghiệp có thể cho các đội marketing, kỹ thuật và tài chính sử dụng chung gateway nhưng áp dụng những policy khác nhau.

Gateway cũng có thể ghi nhận application ID và cost center để biết bộ phận nào đang sử dụng model nào và tiêu thụ bao nhiêu tài nguyên.

## Những trade-off cần lưu ý

Thứ nhất, Guardrails không thay thế authentication, authorization, IAM hay data governance. Một prompt không chứa nội dung nguy hiểm không có nghĩa người dùng được quyền truy cập mọi dữ liệu.

Thứ hai, mỗi lần gọi `ApplyGuardrail` tạo thêm một bước xử lý, vì vậy latency và chi phí có thể tăng. Đổi lại, request vi phạm có thể bị chặn trước khi phát sinh model inference.

Thứ ba, gateway tập trung giúp chính sách đồng nhất nhưng cũng trở thành một thành phần quan trọng. Nếu gateway gặp sự cố hoặc cấu hình sai, nhiều ứng dụng AI có thể bị ảnh hưởng cùng lúc.

Đặc biệt, reference implementation trong bài AWS Blog chỉ áp dụng Guardrails cho input. Response từ LLM chưa được kiểm tra lại trước khi trả về người dùng.

Trong các hệ thống nhạy cảm, theo mình cần bổ sung một bước kiểm tra output:

```text
Prompt
↓
Input Guardrail
↓
LLM
↓
Output Guardrail
↓
Safe Response
```

Đây là phần mở rộng dựa trên khả năng của `ApplyGuardrail`, không phải phần đã được triển khai hoàn chỉnh trong mẫu của bài viết.

## Góc nhìn cá nhân

Điểm mình thấy đáng học nhất là cách đưa chính sách an toàn ra khỏi từng ứng dụng và đặt tại một gateway chung.

Cách tiếp cận này phù hợp với doanh nghiệp có nhiều đội và nhiều LLM provider, vì giúp giảm tình trạng mỗi dự án tự xây một lớp bảo vệ khác nhau.

Tuy nhiên, với một ứng dụng nhỏ chỉ sử dụng một model, kiến trúc gồm Amazon ECS, AWS Fargate, Kinesis, Firehose, S3, Glue và Athena có thể phức tạp hơn nhu cầu thực tế.

Bài học mình rút ra là Guardrails không phải “lá chắn tuyệt đối”. Đây là một phần của mô hình defense in depth, cần kết hợp với IAM least privilege, encryption, monitoring, data minimization và kiểm thử thường xuyên.

Nếu mọi người đã sử dụng Amazon Bedrock Guardrails trong thực tế, mọi người đánh giá như thế nào về false positive, latency, chi phí và độ phức tạp vận hành?

Cảm ơn mọi người đã đọc bài!

## Tài liệu tham khảo

- [Safeguard generative AI applications with Amazon Bedrock Guardrails](https://aws.amazon.com/blogs/machine-learning/safeguard-generative-ai-applications-with-amazon-bedrock-guardrails/) — AWS Artificial Intelligence Blog, 15/01/2026.
- [Amazon Bedrock Guardrails](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails.html)
- [ApplyGuardrail API](https://docs.aws.amazon.com/bedrock/latest/userguide/guardrails-use-independent-api.html)

---

[Trước](/vi/3-blogsposted/3.1-blog1/) | [Quay lại Blogs Posted](/vi/3-blogsposted/) | [Tiếp](/vi/3-blogsposted/3.3-blog3/)
