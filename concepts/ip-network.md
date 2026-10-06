---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: true
_icon: box
aliases: ["IP network", "mạng IP", "mạng"]
Related to:
  - "[[ip-address]]"
  - "[[broadcast-domain]]"
  - "[[network-mask]]"
  - "[[router]]"
  - "[[host]]"
  - "[[default-gateway]]"
  - "[[subnet-cung-mot-khoi-van-phai-qua-router]]"
Belongs to: []
Has:
  - "[[broadcast-domain]]"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---
# Mạng IP (IP network)

**Mạng IP (IP network)** là một **nhóm địa chỉ IP cùng chung một miền quảng bá (broadcast domain)** và **không cần bộ định tuyến (router) để giao tiếp với nhau**.

Nói cách khác: các thiết bị trong cùng một mạng IP "nghe thấy" nhau trực tiếp, trao đổi dữ liệu được với nhau mà không cần qua thiết bị trung gian nào. Khi một thiết bị muốn nói chuyện với một địa chỉ **thuộc mạng khác**, nó bắt buộc phải gửi gói tin tới **bộ định tuyến (router)** — cụ thể là **cổng mặc định (default gateway)** — và bộ định tuyến (router) chuyển tiếp dựa trên **bảng định tuyến (routing table)**.

## Ba quy tắc nền tảng

Ba quy tắc này mô tả ranh giới của một mạng IP và là kiến thức nền mà người làm mạng phải nắm chắc:

1. **Một mạng con (subnet) = một miền quảng bá (broadcast domain) = một VLAN.** (VLAN – Virtual Local Area Network – địa chỉ mạng LAN ảo, tức một mạng LAN logic được tách bằng cấu hình thay vì bằng cáp.)
2. **Các địa chỉ IP trong cùng một mạng con (subnet) giao tiếp trực tiếp** qua chuyển mạch Ethernet (Ethernet switching), không bị bộ định tuyến (router) ngăn cách.
3. **Các địa chỉ IP thuộc các mạng con (subnet) khác nhau bị ngăn cách bởi một hoặc nhiều bộ định tuyến (router)**, và giao tiếp với nhau thông qua định tuyến IP (IP routing).

## Chia một khối thành nhiều mạng con (subnet) không tạo kết nối trực tiếp

Một hiểu nhầm phổ biến: vì các mạng con (subnet) được chia ra từ **cùng một khối địa chỉ** (khối được cấp, hay allocation), nhiều người tưởng chúng vẫn "gần nhau". Thực tế ranh giới do **mặt nạ mạng (network mask)** quyết định, không do khối gốc:

- **Khối được cấp (allocation)** là dải địa chỉ được cấp cho tổ chức (ví dụ `192.168.1.0/24`).
- **Mạng IP / mạng con (subnet)** là một dải con với **ranh giới do mặt nạ mạng (network mask) quyết định** (ví dụ `192.168.1.0/26` và `192.168.1.64/26`).

Hai host thuộc hai mạng con (subnet) khác nhau — dù cùng nằm trong `192.168.1.0/24` — vẫn **bắt buộc đi qua bộ định tuyến (router)**. Xem ví dụ số cụ thể tại [[chia-mot-khoi-lam-bon-subnet-26]] và lập luận đầy đủ tại [[subnet-cung-mot-khoi-van-phai-qua-router]].

### Ai quyết định "cùng mạng hay khác mạng"?

[[host]] **tự quyết định** bằng **phép AND bit**: áp mặt nạ mạng (network mask) của chính mình lên cả địa chỉ nguồn và địa chỉ đích, rồi so hai kết quả [1].

- **Bằng nhau →** cùng mạng con (subnet): dùng ARP lấy địa chỉ MAC (MAC address) đích rồi gửi trực tiếp qua [[switch]].
- **Khác nhau →** khác mạng con (subnet): gửi cho [[default-gateway]] để [[router]] chuyển tiếp.

Điểm đáng chú ý: quyết định này **dựa trên mặt nạ mạng (network mask), không dựa trên vị trí vật lý**. Hai host cắm chung một bộ chuyển mạch (switch) nhưng khác mạng con (subnet) vẫn phải gửi nhau qua cổng mặc định (default gateway) [1].

## Phép so sánh với điện thoại

Hãy hình dung điện thoại bàn: gọi một số trong cùng khu vực thì bấm số trực tiếp; gọi số ở khu vực khác thì phải quay **mã vùng** trước. Mạng IP vận hành tương tự — cùng mạng thì "bấm thẳng" (giao tiếp trực tiếp), khác mạng thì phải "quay mã vùng" (đi qua bộ định tuyến (router)).

Xem thêm: [[broadcast-domain]], [[ip-address]], [[network-mask]], [[router]], [[host]], [[default-gateway]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.
