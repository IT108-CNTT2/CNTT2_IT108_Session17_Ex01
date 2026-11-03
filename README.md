# Phân tích lỗi HTTP 500 khi truy cập LMS PTIT

## Phần 1: Phân tích

**Nam sai:** Lỗi `500 Internal Server Error` là lỗi **phía server**, không phải do DNS không tìm thấy IP.

- **200 OK:** Server xử lý thành công.
- **404 Not Found:** Server không tìm thấy tài nguyên được yêu cầu.
- **500 Internal Server Error:** Server gặp lỗi khi xử lý yêu cầu.

Nếu DNS không phân giải được tên miền, trình duyệt thường báo lỗi DNS, **không nhận mã HTTP 500**. Vì vậy, đổi DNS sang `8.8.8.8` không phải cách khắc phục trực tiếp lỗi này.

## Phần 2: Luồng HTTP Request

```text
Nhập lms.ptit.edu.vn
        ↓
DNS Lookup → Nhận địa chỉ IP
        ↓
Kết nối TCP → TLS (HTTPS)
        ↓
Trình duyệt gửi HTTP Request
        ↓
Server nhận và xử lý yêu cầu
        ↓
Server gặp lỗi nội bộ
        ↓
HTTP Response: 500 Internal Server Error
        ↓
Trình duyệt hiển thị lỗi 500
```

**Kết luận:** Nam đã nhận được phản hồi HTTP 500 nên yêu cầu đã đến một máy chủ có khả năng trả lời HTTP. Vấn đề cần kiểm tra chủ yếu ở **server hoặc hệ thống phía server**, không nên vội gọi thợ sửa mạng.
