---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["IPv6", "Internet Protocol version 6"]
Related to:
  - "[[ip-address]]"
  - "[[cidr]]"
  - "[[network-address-translation]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# IPv6

**IPv6 (Internet Protocol version 6)** là phiên bản giao thức IP dùng địa chỉ **128 bit**, thay vì 32 bit như IPv4. Không gian địa chỉ IPv6 chứa khoảng **3,4 × 10³⁸** địa chỉ — đủ lớn để giải quyết tận gốc vấn đề cạn kiệt của IPv4 [1].

## Vì sao IPv6 giải quyết "tận gốc"

IPv4 hữu hạn ở `2³²` địa chỉ, và các giải pháp kéo dài tuổi thọ (CIDR, NAT, CGNAT) chỉ là giảm nhẹ triệu chứng. IPv6 tăng không gian địa chỉ lên `2¹²⁸` — một con số lớn đến mức có thể cấp địa chỉ public cho **mọi thiết bị**, bỏ được nhu cầu NAT.

## Mốc chuẩn hoá

- **RFC 2460** — chuẩn hoá IPv6 năm **1998**.
- **RFC 8200** — hợp nhất lại đặc tả IPv6 năm **2017** [1].

## Điều chuyển tiếp được từ IPv4

✦ *Diễn giải:* khái niệm **prefix** và **tổng hợp tuyến (route aggregation)** của [[cidr]] **vẫn được dùng trong IPv6**. Nhờ vậy, kỹ năng tính prefix và chia mạng con (subnetting) ở IPv4 chuyển sang được, chỉ khác ở độ dài địa chỉ (128 bit thay vì 32 bit) [1].

Xem thêm: [[ip-address]], [[cidr]], [[network-address-translation]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.