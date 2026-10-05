---
type: Insight
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: lightbulb
claim: "NAT phá vỡ kết nối đầu-cuối (end-to-end), khiến các ứng dụng nhúng địa chỉ IP trong phần dữ liệu (payload) khó hoạt động đúng"
confidence: medium
Related to:
  - "[[network-address-translation]]"
  - "[[private-ip-address]]"
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# NAT phá vỡ kết nối đầu-cuối (end-to-end), khiến các ứng dụng nhúng địa chỉ IP trong phần dữ liệu (payload) khó hoạt động đúng

## Bối cảnh

**[[network-address-translation]]** ra đời như một giải pháp kéo dài tuổi thọ IPv4: nhiều thiết bị dùng chung một địa chỉ public. Nhưng nó không miễn phí — cái giá là mất một tính chất nền tảng của kiến trúc IP.

## Luận điểm

NAT **phá vỡ kết nối đầu-cuối (end-to-end)**. Kiến trúc IP nguyên thuỷ giả định hai đầu cuối nói chuyện với nhau bằng địa chỉ thật của chúng, xuyên suốt đường truyền. NAT chèn một tầng viết lại địa chỉ ở giữa, nên giả định đó không còn đúng.

Hệ quả trực tiếp: **các ứng dụng đặt địa chỉ IP ngay trong phần dữ liệu (payload) của gói tin trở nên khó hoạt động đúng**, vì bộ NAT chỉ viết lại địa chỉ trong **phần tiêu đề (header) của gói tin**, không viết lại địa chỉ đã được nhúng trong phần dữ liệu (payload) [1].

## Ví dụ điển hình

Nguồn nêu hai ví dụ: **VoIP** (thoại qua IP) và **IPsec** (bộ giao thức bảo mật cho IP). Cả hai đều mang thông tin địa chỉ trong phần dữ liệu (payload)/được ký/xác thực, nên khi đi qua NAT có thể bị sai lệch hoặc không khớp với địa chỉ đã viết lại [1].

## Phạm vi / ngoại lệ

- Đây là **đánh đổi (trade-off)**, không phải lỗi: cái giá này đổi lấy việc tiết kiệm địa chỉ public ở quy mô cực lớn.
- **CGNAT** (NAT cấp nhà mạng) làm vấn đề trầm trọng hơn vì khách hàng có thể bị NAT **hai lớp**, khiến địa chỉ thấy từ ngoài càng khác xa địa chỉ gốc [1].
- **[[ipv6]]** loại bỏ nhu cầu NAT bằng cách cấp đủ địa chỉ public cho mọi thiết bị.

Xem thêm: [[network-address-translation]], [[private-ip-address]], [[ipv6]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.