---
title: "Tính năng mới gây lỗi thì có cần rollback cả bản deploy không?"
date: 2026-07-22
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
---

## AWS DEVOPS | Tính năng mới gây lỗi thì có cần rollback cả bản deploy không?

| Thông tin | Chi tiết |
|---|---|
| Ngày đăng | 22/07/2026 |
| Trạng thái | Đã đăng |
| Nền tảng | AWS Study Group - Facebook Group |
| Bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2221213298643630) |
| Chủ đề | Feature flag, AWS AppConfig, AWS DevOps Agent, LaunchDarkly và xử lý sự cố sau release |

Chào mọi người,

Chắc nhiều bạn từng gặp cảnh vừa deploy xong thì error rate tăng, một API bắt đầu chậm hoặc có chức năng nào đó không dùng được.

Phản ứng thường thấy nhất là rollback ngay về phiên bản cũ.

Cách này an toàn, nhưng đôi khi hơi “quá tay”. Nếu bản deploy có 10 thay đổi mà chỉ một tính năng mới gây lỗi, rollback toàn bộ cũng đồng nghĩa với việc gỡ luôn 9 phần còn lại đang chạy bình thường.

Lúc này, **feature flag** có thể là một lựa chọn hợp lý hơn.

Có thể hiểu đơn giản feature flag là một công tắc bật/tắt tính năng:

```text
new_checkout = ON / OFF
```

Khi phát hiện luồng thanh toán mới có vấn đề, đội vận hành chỉ cần tắt flag để người dùng quay lại luồng cũ, không phải build và deploy lại toàn bộ ứng dụng.

Một quy trình xử lý có thể như sau:

```text
CloudWatch phát hiện lỗi
        ↓
Kiểm tra thay đổi vừa được phát hành
        ↓
Xác định feature flag có liên quan
        ↓
Tắt flag
        ↓
Theo dõi error rate và latency
```

AWS AppConfig hỗ trợ quản lý feature flag, triển khai dần theo từng nhóm người dùng và theo dõi CloudWatch alarm. Nếu alarm được kích hoạt trong lúc rollout, AppConfig có thể tự động đưa cấu hình về phiên bản trước.

AWS cũng giới thiệu cách kết nối AWS DevOps Agent với LaunchDarkly. Trong lúc điều tra sự cố, agent có thể lấy trạng thái flag, targeting rule và tỷ lệ rollout để đưa ra gợi ý cho kỹ sư. Điểm quan trọng là agent hỗ trợ thu thập thông tin và đề xuất, chứ không nhất thiết phải tự ý tắt tính năng.

![So sánh rollback toàn bộ deployment với xử lý sự cố bằng feature flag](/images/blogs/blog5-feature-flag-incident-response.webp)

*Figure 1. Comparison between rolling back an entire deployment and disabling only the affected feature — So sánh rollback toàn bộ deployment với tắt riêng tính năng lỗi và quy trình xử lý sự cố có kiểm soát bằng CloudWatch, AWS DevOps Agent và LaunchDarkly.*

Tuy nhiên, feature flag không phải nút “cứu mọi sự cố”.

Nếu nguyên nhân nằm ở database, network hoặc dịch vụ bên ngoài thì tắt flag có thể không giải quyết được gì. Dùng quá nhiều flag mà không dọn dẹp cũng khiến code ngày càng nhiều nhánh và khó kiểm thử.

Theo mình, feature flag hữu ích nhất khi được xem như một **công tắc khẩn cấp có kiểm soát**: giúp giảm ảnh hưởng tới người dùng trước, còn nguyên nhân gốc vẫn phải được tìm và sửa sau đó.

Còn mọi người, khi production gặp lỗi ngay sau release, mọi người thường rollback cả phiên bản hay tắt riêng tính năng trước?

## Nguồn tham khảo

- [AWS DevOps Blog – Feature flag orchestration with AWS DevOps Agent and LaunchDarkly](https://aws.amazon.com/blogs/devops/feature-flag-orchestration-with-aws-devops-agent-and-launchdarkly/)
- [AWS AppConfig – What is AWS AppConfig?](https://docs.aws.amazon.com/appconfig/latest/userguide/what-is-appconfig.html)

---

[Trước](/vi/3-blogsposted/3.4-blog4/) | [Quay lại Blogs Posted](/vi/3-blogsposted/)
