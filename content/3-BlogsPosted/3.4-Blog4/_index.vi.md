---
title: "Vì sao deploy web vẫn có thể làm người dùng gặp lỗi 502?"
date: 2026-07-22
weight: 4
chapter: false
pre: " <b> 3.4. </b> "
---

## AWS DEVOPS | Vì sao deploy web vẫn có thể làm người dùng gặp lỗi 502?

| Thông tin | Chi tiết |
|---|---|
| Ngày đăng | 22/07/2026 |
| Trạng thái | Đã đăng |
| Nền tảng | AWS Study Group - Facebook Group |
| Bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2220504978714462) |
| Chủ đề | Amazon ECS deployment, graceful shutdown, load balancer connection draining và rollback |

Chào mọi người,

Một tình huống khá khó chịu khi vận hành web là: phiên bản mới đã deploy thành công, container mới cũng báo healthy, nhưng trong vài giây chuyển đổi vẫn có người dùng gặp lỗi `502`, request bị ngắt hoặc tác vụ đang chạy dở bị mất.

Nguyên nhân đôi khi không nằm ở code mới, mà nằm ở cách container cũ được tắt.

Giả sử một ứng dụng đang chạy trên Amazon ECS và nhận traffic qua Application Load Balancer:

```text
User
  ↓
Application Load Balancer
  ↓
ECS Task cũ + ECS Task mới
```

Khi deploy, ECS khởi động task mới và dần dừng task cũ. Nhưng task cũ có thể vẫn đang xử lý một request dài, giữ kết nối database hoặc chạy một công việc chưa hoàn thành.

Nếu process bị tắt ngay, những request đó sẽ bị ngắt giữa chừng.

![Vòng đời rolling deployment trên Amazon ECS với task cũ và task mới](/images/blogs/blog4-ecs-deployment-lifecycle.webp)

*Figure 1. Amazon ECS rolling deployment lifecycle while traffic transitions from the old task to the new task — Vòng đời rolling deployment khi traffic chuyển từ task cũ sang task mới.*

## Health check xanh chưa có nghĩa deploy đã an toàn

Health check chủ yếu trả lời câu hỏi:

> Container mới đã sẵn sàng nhận request chưa?

Nó không đảm bảo container cũ đã xử lý xong toàn bộ request trước khi bị dừng.

Để giải quyết phần này, Application Load Balancer sử dụng **deregistration delay**. Khi một target bị gỡ khỏi target group, load balancer ngừng gửi request mới nhưng vẫn cho các request đang chạy thêm thời gian để hoàn thành. Giá trị mặc định hiện tại là 300 giây và có thể điều chỉnh theo đặc điểm của ứng dụng.

```text
Ngừng nhận request mới
          ↓
Chờ request đang chạy hoàn thành
          ↓
Dừng container
```

Không phải cứ đặt thời gian thật dài là tốt. Nếu phần lớn request chỉ mất vài trăm mili giây, chờ 5 phút có thể làm deployment và scale-in chậm không cần thiết.

## Ứng dụng cũng phải biết cách tự dừng

Khi ECS dừng một task, container trước tiên nhận tín hiệu `SIGTERM`. Nếu process không thoát trong khoảng thời gian cho phép, ECS sẽ gửi `SIGKILL` để buộc dừng. Khoảng chờ mặc định là 30 giây và có thể cấu hình bằng `stopTimeout`.

Ứng dụng nên xử lý `SIGTERM` bằng cách:

- Ngừng nhận request mới.
- Hoàn thành các request đang chạy.
- Đóng kết nối database.
- Flush log hoặc dữ liệu còn trong bộ nhớ.
- Sau đó mới thoát process.

Nếu application không xử lý graceful shutdown, tăng deregistration delay cũng chưa chắc giải quyết được vấn đề.

![Luồng ALB connection draining và graceful shutdown trên Amazon ECS](/images/blogs/blog4-alb-graceful-shutdown-flow.webp)

*Figure 2. Coordination among Application Load Balancer connection draining, Amazon ECS task stopping, and application graceful shutdown — Sự phối hợp giữa ALB connection draining, quá trình dừng ECS task và graceful shutdown của ứng dụng.*

## Container mới cũng cần thời gian khởi động

Một lỗi ngược lại là container vừa chạy đã bị health check đánh dấu unhealthy vì ứng dụng còn đang kết nối database, tải configuration hoặc warm up cache.

Amazon ECS có `healthCheckGracePeriodSeconds`, cho phép tạm thời bỏ qua health check thất bại trong giai đoạn task mới khởi động.

Tuy nhiên, grace period quá dài cũng có mặt trái: một container thực sự bị lỗi có thể tồn tại lâu hơn trước khi được thay thế.

## Khi bản deploy mới thật sự lỗi

ECS Deployment Circuit Breaker có thể phát hiện deployment không đạt trạng thái ổn định và tự động rollback về deployment trước đó.

Nó hữu ích khi container không khởi động được hoặc liên tục fail health check. Nhưng circuit breaker không thay thế monitoring ở tầng ứng dụng.

![Các cơ chế bảo vệ deployment trên Amazon ECS với health check monitoring và rollback](/images/blogs/blog4-ecs-deployment-safeguards.webp)

*Figure 3. Amazon ECS deployment safeguards combining startup grace periods, health checks, monitoring, circuit breaker detection, and rollback — Các lớp bảo vệ deployment gồm grace period, health check, monitoring, circuit breaker và rollback.*

Một phiên bản mới vẫn có thể healthy về mặt kỹ thuật nhưng:

- Response chậm hơn.
- Error rate tăng.
- Trả dữ liệu sai.
- Làm hỏng một chức năng ít người sử dụng.

Vì vậy, deploy an toàn vẫn cần theo dõi latency, HTTP 5xx, error rate và metric nghiệp vụ sau khi release.

Điểm mình rút ra là “zero-downtime deployment” không đến từ một setting duy nhất. Nó là sự phối hợp giữa load balancer, ECS scheduler, health check và chính cách ứng dụng phản ứng khi bị dừng.

Nhiều lúc hệ thống không hỏng vì phiên bản mới, mà vì phiên bản cũ chưa kịp nói lời tạm biệt 😄

Mọi người thường kiểm tra graceful shutdown như một phần của pipeline deployment hay chỉ phát hiện ra khi production bắt đầu rớt request?

## Nguồn tham khảo

- [Amazon ECS – Optimize load balancer connection draining](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/load-balancer-connection-draining.html)
- [Application Load Balancer – Deregistration delay](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/edit-target-group-attributes.html)
- [Amazon ECS deployment circuit breaker](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/deployment-circuit-breaker.html)

---

[Trước](/vi/3-blogsposted/3.3-blog3/) | [Quay lại Blogs Posted](/vi/3-blogsposted/) | [Tiếp](/vi/3-blogsposted/3.5-blog5/)
