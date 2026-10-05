---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["classless", "classless addressing", "địa chỉ không phân lớp"]
Related to:
  - "[[classful-addressing]]"
  - "[[vlsm]]"
  - "[[cidr]]"
  - "[[network-mask]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Địa chỉ không phân lớp (classless addressing)

**Địa chỉ không phân lớp (classless addressing)** là mô hình cấp phát địa chỉ IP dùng **các khối có độ dài thay đổi**, không thuộc lớp nào. Điểm khác biệt cốt lõi so với mô hình có lớp: **địa chỉ không còn ngụ ý mặt nạ mạng (network mask) — ta phải khai báo mặt nạ mạng (network mask) một cách tường minh** [2].

## Điều gì thay đổi so với classful

- Mặt nạ mạng (network mask) **không còn bị ràng buộc theo lớp**: các mặt nạ mạng (network mask) `/8`, `/16`, `/24` có thể được gán cho bất kỳ địa chỉ nào, kể cả những địa chỉ theo truyền thống thuộc dải lớp A, B hoặc C.
- Không còn bị giới hạn ở ba lựa chọn `/8, /16, /24` — **bất kỳ độ dài prefix nào** đều dùng được.
- **Không có ranh giới cố định** giữa phần định danh mạng và phần định danh host; ranh giới do mặt nạ mạng (network mask) quyết định.

Ví dụ đối chiếu:

| | Classful | Classless |
|---|---|---|
| `10.1.1.2` | Luôn là lớp A → mặt nạ mạng (network mask) `255.0.0.0` (`/8`) | Có thể mang mặt nạ mạng (network mask) bất kỳ, ví dụ `255.255.224.0` (`/19`) |
| Suy mặt nạ mạng (network mask) từ địa chỉ? | Có | **Không** — phải được cho biết rõ |

## Hai kỹ thuật hiện thực hoá classless

Classless đứng trên hai kỹ thuật cụ thể:

1. **[[vlsm]]** (Variable-Length Subnet Masking – mặt nạ mạng con có độ dài thay đổi): cho phép chia **một khối thành các mạng con (subnet) có kích thước khác nhau**, mỗi mạng con (subnet) có prefix riêng.
2. **[[cidr]]** (Classless Inter-Domain Routing – định tuyến liên miền không phân lớp): áp ý tưởng đó lên **toàn bộ Internet** với prefix độ dài tuỳ ý và gom tuyến (route aggregation — tập hợp nhiều tuyến thành một tuyến tổng hợp).

## Vì sao classless ra đời

Cách đánh địa chỉ có lớp lãng phí một lượng lớn địa chỉ vào những khối quá lớn cấp cho nhu cầu của vài host. Classless cho phép **kiểm soát kích thước mạng IP theo đúng nhu cầu** — đây chính là điều ta gọi là **chia mạng con (subnetting)** [2].

Xem thêm: [[classful-addressing]], [[vlsm]], [[cidr]], [[network-mask]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.