---
title: "Một hành động an toàn chưa chắc tạo thành một chuỗi an toàn"
date: 2026-07-21
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

## AI Agent Safety | Một hành động an toàn chưa chắc tạo thành một chuỗi an toàn

| Thông tin | Chi tiết |
|---|---|
| Ngày đăng | 21/07/2026 |
| Trạng thái | Đã đăng |
| Nền tảng | AWS Study Group - Facebook Group |
| Bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2220015812096712) |
| Chủ đề | AI agent safety, trajectory analysis và sandboxed code execution |

Chào mọi người,

Khi đánh giá độ an toàn của AI agent, chúng ta thường nhìn vào prompt, câu trả lời cuối cùng hoặc từng lần agent gọi công cụ.

Nhưng giả sử agent thực hiện ba bước:

1. Đọc một file nội bộ.
2. Tóm tắt nội dung.
3. Gửi bản tóm tắt đến một dịch vụ bên ngoài.

Mỗi hành động khi đứng riêng có thể trông hợp lệ, nhưng cả chuỗi lại có nguy cơ làm rò rỉ dữ liệu.

Đây là lý do một số nghiên cứu gần đây chuyển từ việc đánh giá từng hành động sang phân tích toàn bộ **trajectory** — lịch sử gồm suy luận, tool call và phản hồi từ môi trường.

ATBench xây dựng 1.000 trajectory an toàn và không an toàn để kiểm tra khả năng phát hiện rủi ro trong các tương tác nhiều bước. HINTBench còn chỉ ra rằng mô hình có thể nhận biết một phiên đang nguy hiểm, nhưng lại khó xác định chính xác hành động nào bắt đầu gây ra vấn đề.

Điều này cho thấy chỉ lọc prompt hoặc chặn một vài command nhạy cảm là chưa đủ. Một hệ thống bảo vệ agent nên kết hợp:

- Giới hạn quyền và công cụ được sử dụng.
- Chạy code trong môi trường cách ly.
- Kiểm tra các thao tác quan trọng trước khi thực thi.
- Lưu lại toàn bộ trajectory để phát hiện rủi ro tích lũy.

Amazon Bedrock AgentCore Code Interpreter, chẳng hạn, cho phép chạy code trong môi trường container được cách ly và hỗ trợ các chế độ mạng như Sandbox, Public và VPC. Tuy nhiên, sandbox chỉ giúp giới hạn hậu quả; nó không tự xác định được agent đang đi sai mục tiêu hay không.

![Luồng thực thi công cụ của AI agent với AgentCore Code Interpreter](/images/blogs/blog3-agentcore-code-interpreter-flow.webp)

*Figure 1. AI agent tool execution flow with a Code Interpreter session and observability telemetry — Luồng agent thực thi công cụ và gửi telemetry phục vụ quan sát.*

Điểm mình rút ra là AI agent safety không còn chỉ là kiểm soát **nội dung agent nói**, mà còn phải theo dõi **agent đã làm gì và chuỗi hành động đó đang dẫn tới đâu**.

Theo mọi người, khi triển khai AI agent, nên ưu tiên sandbox chặt từ đầu hay đầu tư nhiều hơn vào hệ thống theo dõi trajectory?

## Nguồn tham khảo

- [ATBench](https://arxiv.org/abs/2604.02022)
- [HINTBench](https://arxiv.org/abs/2604.13954)
- [Amazon Bedrock AgentCore Code Interpreter](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/code-interpreter-tool.html)

---

[Trước](/vi/3-blogsposted/3.2-blog2/) | [Quay lại Blogs Posted](/vi/3-blogsposted/) | [Tiếp](/vi/3-blogsposted/3.4-blog4/)
