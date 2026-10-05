---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["host", "thiết bị đầu cuối", "máy trạm"]
Related to:
  - "[[ip-address]]"
  - "[[network-mask]]"
  - "[[default-gateway]]"
  - "[[ip-network]]"
  - "[[switch]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Host (thiết bị đầu cuối)

**Host (thiết bị đầu cuối)** là **máy tính, điện thoại, máy chủ, hay bất kỳ thiết bị nào có địa chỉ IP** [1]. Trong mạng IP, host chính là điểm đầu cuối (endpoint) gửi và nhận dữ liệu.

## Host dùng IP cần gì

Một host cần tối thiểu **ba thông số** để dùng IP bình thường [1]:

- **Địa chỉ IP** — định danh của host trong mạng.
- **Mặt nạ mạng (network mask)** — cho biết phần nào của địa chỉ là mạng, phần nào là host.
- **Cổng mặc định (default gateway)** — nơi gửi gói tin khi đích ở ngoài mạng.

Thiếu cổng mặc định (default gateway), host vẫn nói chuyện được với host **cùng mạng con (subnet)**, nhưng **không ra được ngoài mạng** [1].

## Quyết định "cùng mạng hay khác mạng" là của host

Host **tự quyết định** một đích là cùng mạng hay khác mạng bằng **phép AND bit**: nó áp mặt nạ mạng (network mask) của chính mình lên cả địa chỉ nguồn và địa chỉ đích rồi so hai kết quả [1].

- **Bằng nhau →** cùng mạng con (subnet): host dùng ARP lấy MAC đích rồi gửi trực tiếp qua **[[switch]]**.
- **Khác nhau →** khác mạng con (subnet): host gửi cho **[[default-gateway]]** để **[[router]]** chuyển tiếp.

Điểm đáng chú ý: quyết định này **dựa trên mặt nạ mạng (network mask), không dựa trên vị trí vật lý**. Hai host cắm chung một bộ chuyển mạch (switch) nhưng khác mạng con (subnet) vẫn phải gửi nhau qua cổng mặc định (default gateway) [1].

Xem thêm: [[ip-address]], [[network-mask]], [[default-gateway]], [[ip-network]], [[switch]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.
