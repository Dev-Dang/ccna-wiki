---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["CIDR", "Classless Inter-Domain Routing", "định tuyến liên miền không phân lớp", "prefix notation", "dấu gạch chéo"]
Related to:
  - "[[classless-addressing]]"
  - "[[vlsm]]"
  - "[[network-mask]]"
Belongs to:
  - "[[classless-addressing]]"
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# CIDR (Classless Inter-Domain Routing)

**CIDR (Classless Inter-Domain Routing – định tuyến liên miền không phân lớp)** là kỹ thuật áp ý tưởng không phân lớp lên **toàn bộ Internet**: cho phép dùng **prefix độ dài tuỳ ý** và **gom tuyến (route aggregation)**.

## Ký hiệu "gạch chéo" (slash notation / prefix)

CIDR giới thiệu cách viết địa chỉ kèm độ dài prefix: **`/n`** nghĩa là **`n` bit đầu là phần mạng**, `(32 − n)` bit còn lại là phần host.

| Prefix | Mặt nạ mạng (network mask) tương đương | Bit host | Số địa chỉ |
|---|---|---|---|
| `/8` | `255.0.0.0` | 24 | 16.777.216 |
| `/16` | `255.255.0.0` | 16 | 65.536 |
| `/24` | `255.255.255.0` | 8 | 256 |
| `/26` | `255.255.255.192` | 6 | 64 |
| `/30` | `255.255.255.252` | 2 | 4 |

## Vì sao CIDR ra đời

CIDR ra đời năm **1993** (**RFC 1519**, sau được thay bởi **RFC 4632**) với mục tiêu **hãm tốc độ phình của bảng định tuyến (routing table)** [1].

Cơ chế: thay vì quảng bá **một tuyến cho mỗi khách hàng**, nhà cung cấp quảng bá **một tuyến tổng hợp duy nhất** đại diện cho nhiều khách hàng có khối địa chỉ liền kề. Việc gom nhiều khối rời rạc thành một tuyến duy nhất làm bảng định tuyến (routing table) toàn cầu nhỏ hơn nhiều.

Ví dụ: thay vì quảng bá bốn tuyến `200.1.0.0/24`, `200.1.1.0/24`, `200.1.2.0/24`, `200.1.3.0/24`, chỉ cần một tuyến `200.1.0.0/22` — miễn là chúng liền kề và cùng thuộc một khách hàng/nhà cung cấp.

## Ranh giới cần phân biệt

- **CIDR** là khái niệm ở **tầng định tuyến toàn cầu** (gom tuyến (route aggregation) để thu nhỏ bảng định tuyến (routing table)).
- **[[vlsm]]** là khái niệm ở **tầng chia khối nội bộ** (cắt một khối thành các mạng con (subnet) cỡ khác nhau).

Cả hai đều dựa trên cùng nền tảng **prefix độ dài tùy ý** của [[classless-addressing]], và khái niệm prefix của CIDR **vẫn được dùng trong IPv6** — nên kỹ năng tính prefix ở IPv4 chuyển sang được [1].

Xem thêm: [[classless-addressing]], [[vlsm]], [[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.