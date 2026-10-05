---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["khối địa chỉ đặc biệt", "special-use addresses", "special IP blocks", "RFC 6890"]
Related to:
  - "[[ip-address]]"
  - "[[private-ip-address]]"
  - "[[loopback-address]]"
  - "[[ip-network]]"
  - "[[network-address-translation]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Khối địa chỉ đặc biệt (special-use address blocks)

Không phải mọi dải địa chỉ IPv4 đều dùng để cấp cho máy thật. Một số **khối địa chỉ đặc biệt (special-use address blocks)** được dành riêng cho mục đích cụ thể và **không được định tuyến trên Internet toàn cầu** [1].

## Bảng khối địa chỉ đặc biệt

| Khối | Vai trò | Ghi chú |
|---|---|---|
| `0.0.0.0/8` | **Dự trữ (reserved)** | `0.0.0.0` xuất hiện trong bảng định tuyến (routing table) như **tuyến mặc định (default route)** — nghĩa "khi không khớp tuyến nào khác" |
| `127.0.0.0/8` | **Loopback** | Địa chỉ để thiết bị nói chuyện với chính nó; `127.0.0.1` = localhost. Xem [[loopback-address]] |
| `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` | **Private (RFC 1918)** | Dùng trong mạng nội bộ, cần **NAT** để ra Internet. Xem [[private-ip-address]] |
| `100.64.0.0/10` | **Shared address space (RFC 6598)** | Dùng giữa nhà mạng và thiết bị **CGNAT (Carrier-Grade NAT)** |
| `169.254.0.0/16` | **Link-local (RFC 3927)** | Tự gán khi host **không lấy được địa chỉ từ DHCP**; **bộ định tuyến (router) không chuyển tiếp** |
| `224.0.0.0/4` | **Multicast (lớp D)** | Dải dành cho multicast |
| `240.0.0.0/4` | **Dự trữ (lớp E)** | Dải để dành cho tương lai |

*(Bảng tổng hợp từ tài liệu nguồn; phần giải thích RFC 1918, CGNAT và loopback là diễn giải nối từ các note liên quan [[private-ip-address]], [[network-address-translation]], [[loopback-address]].)*

## Vì sao phải biết các khối này

- **Đọc log và cấu hình:** thấy `169.254.x.x` là dấu hiệu host **không lấy được địa chỉ từ DHCP**, chứ không phải "mạng chết" [1].
- **Phân biệt `127.0.0.0/8` với interface loopback của bộ định tuyến (router):** `127.0.0.0/8` là địa chỉ loopback **của máy** (gói tin không ra khỏi máy); còn **interface loopback** trên bộ định tuyến (router) là một **interface ảo luôn ở trạng thái up, thường được gán một địa chỉ /32** — hai thứ khác nhau, đừng nhầm. Xem [[loopback-address]] [1].
- **Nhận diện `0.0.0.0` trong bảng định tuyến (routing table)** là **tuyến mặc định (default route)**, không phải một địa chỉ host [1].

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.