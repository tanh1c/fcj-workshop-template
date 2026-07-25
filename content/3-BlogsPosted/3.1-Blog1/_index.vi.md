---
title: "Khi data pipeline có quá nhiều bản ‘final’"
date: 2026-07-17
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---

## AWS Architecture Blog | Khi data pipeline có quá nhiều bản “final”

| Thông tin | Chi tiết |
|---|---|
| Ngày đăng | 17/07/2026 |
| Trạng thái | Đã đăng |
| Nền tảng | AWS Study Group - Facebook Group |
| Bài đăng | [Xem bài viết trên Facebook](https://www.facebook.com/groups/awsstudygroupfcj/?multi_permalinks=2216239979140962) |
| Bài AWS tham khảo | *Specification-driven composition for flexible data workflows* — AWS Architecture Blog |

Chào mọi người,

Chắc nhiều bạn từng gặp cảnh một project có các file như `clean_data.py`, `clean_data_v2.py`, `clean_data_final.py` rồi cuối cùng chẳng ai nhớ file nào mới là bản đúng.

Mình vừa đọc bài **“Specification-driven composition for flexible data workflows”** trên AWS Architecture Blog. Bài viết đưa ra một cách xử lý khá hay: thay vì viết riêng một pipeline cho từng bộ dữ liệu, chúng ta mô tả các bước cần làm bằng file JSON hoặc YAML, sau đó hệ thống tự tạo workflow tương ứng.

Ví dụ, file cấu hình chỉ cần mô tả:

```text
order_date → format_date
amount → normalize_currency
email → validate_email
```

Các chức năng như chuẩn hóa ngày tháng hay kiểm tra email được viết và kiểm thử một lần, sau đó dùng lại cho nhiều pipeline khác nhau.

![Specification-Driven Composition design pattern](/images/blogs/blog1-specification-driven-design-pattern.webp)

*Figure 1. Specification-Driven Composition design pattern — Mẫu thiết kế composition dựa trên specification.*

Luồng kiến trúc trong bài có thể hiểu đơn giản như sau:

```text
Specification được tải lên Amazon S3
↓
AWS Lambda kiểm tra cấu hình
↓
Tìm capability trong Amazon OpenSearch Service
↓
AWS Step Functions tạo và điều phối workflow
↓
Các AWS Lambda thực hiện xử lý dữ liệu
```

![AWS implementation of Specification-Driven Composition](/images/blogs/blog1-aws-implementation.webp)

*Figure 2. AWS implementation of Specification-Driven Composition — Cách triển khai pattern trên AWS.*

Lambda đầu tiên đóng vai trò giống một “người lắp ráp”. Nó kiểm tra xem cấu hình có hợp lệ không, các bước xử lý có tồn tại không và input của bước này có phù hợp với output của bước trước hay không.

Sau đó, AWS Step Functions điều phối từng bước và theo dõi trạng thái của workflow. Nếu một bước gặp lỗi, hệ thống có thể retry hoặc chuyển sang luồng xử lý lỗi thay vì để cả pipeline dừng mà không rõ nguyên nhân.

Mình thấy cách làm này phù hợp với những hệ thống phải xử lý nhiều nguồn dữ liệu có cấu trúc gần giống nhau.

Ví dụ, một doanh nghiệp nhận file đơn hàng từ nhiều đối tác. Mỗi bên sử dụng tên cột, định dạng ngày và đơn vị tiền tệ khác nhau. Thay vì copy một script cũ rồi sửa lại cho từng đối tác, đội dữ liệu có thể giữ một bộ capability dùng chung và chỉ viết thêm specification mới.

Tuy nhiên, chuyển từ Python sang JSON không có nghĩa hệ thống tự nhiên trở nên đơn giản.

Doanh nghiệp vẫn phải xây dựng Composer, quản lý phiên bản capability, kiểm tra schema và kiểm soát ai được phép sửa specification. Nếu làm không cẩn thận, chúng ta chỉ chuyển sự hỗn loạn từ các file Python sang các file JSON.

Vấn đề bảo mật cũng đáng lưu ý. Specification chỉ nên được phép sử dụng những capability đã được đăng ký và phê duyệt. Nếu người dùng có thể chèn một Lambda ARN bất kỳ hoặc code tùy ý, hệ thống có thể trở thành nơi thực thi code ngoài kiểm soát.

Theo mình, pattern này chưa cần thiết với một project nhỏ chỉ có vài pipeline. Viết một script hoặc một Step Functions workflow rõ ràng có thể dễ bảo trì hơn.

Nó bắt đầu đáng cân nhắc khi nhiều pipeline đang copy lại cùng một logic, dữ liệu mới được thêm thường xuyên và việc sửa một quy tắc buộc đội phát triển phải cập nhật hàng loạt script.

Điểm mình rút ra sau khi đọc bài là: độ phức tạp không biến mất. Thay vì nằm rải rác trong từng pipeline, nó được gom lại trong một platform chung. Nếu số lượng workflow đủ lớn thì đây là một sự đánh đổi hợp lý, còn nếu áp dụng quá sớm thì có thể thành over-engineering.

Mọi người nghĩ một dự án nên bắt đầu xây platform dùng chung từ lúc nào: khi có nhiều dataset hay khi code bắt đầu bị copy-paste quá nhiều?

Cảm ơn mọi người đã đọc!

## Tài liệu tham khảo

- [Specification-driven composition for flexible data workflows](https://aws.amazon.com/blogs/architecture/specification-driven-composition-for-flexible-data-workflows/) — AWS Architecture Blog.
- [AWS Step Functions Developer Guide](https://docs.aws.amazon.com/step-functions/latest/dg/welcome.html)

---

[Quay lại Blogs Posted](/vi/3-blogsposted/) | [Tiếp](/vi/3-blogsposted/3.2-blog2/)
