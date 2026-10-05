---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["loopback address", "địa chỉ loopback", "localhost", "127.0.0.1"]
Related to:
  - "[[ip-address]]"
  - "[[special-ip-blocks]]"
  - "[[router]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Địa chỉ loopback (loopback address)

**Địa chỉ loopback (loopback address)** là địa chỉ để **thiết bị nói chuyện với chính nó**: gói tin gửi tới địa chỉ này **không ra khỏi máy**, không đi qua cáp mạng [1].

## Dải địa chỉ

- `127.0.0.0/8` là **dải dành riêng cho loopback**, không bao giờ được cấp cho mạng thật [1].
- `127.0.0.1` là địa chỉ loopback quen thuộc nhất, còn có tên **localhost** [1].

## Dùng để làm gì

- **Kiểm tra ngăn xếp TCP/IP:** ping `127.0.0.1` để xác nhận phần mềm mạng trên máy còn hoạt động — không liên quan tới cáp, bộ định tuyến (router) hay cổng mặc định (default gateway) [1].
- **Khi lập trình:** ứng dụng chạy trên máy thường được gọi qua `localhost:<cổng>`, ví dụ `localhost:8080` [1].

## Phân biệt với interface loopback của bộ định tuyến (router)

Đây là chỗ dễ nhầm:

- **Địa chỉ loopback `127.0.0.0/8`** — thuộc về **bản thân máy**; gói tin **không bao giờ rời khỏi máy** [1].
- **Interface loopback trên bộ định tuyến (router)** — là một **interface ảo luôn ở trạng thái up**, không gắn với cổng vật lý nào, thường được gán một địa chỉ **/32**. Nó dùng để bộ định tuyến (router) luôn có một địa chỉ ổn định để tham chiếu (ví dụ làm router-id cho các giao thức định tuyến) [1].

Hai khái niệm **cùng tên "loopback" nhưng khác nhau về bản chất** — đừng nhầm dải `127.0.0.0/8` với interface loopback của bộ định tuyến (router) [1].

Xem thêm: [[ip-address]], [[special-ip-blocks]], [[router]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.