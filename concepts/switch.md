---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: true
_icon: box
aliases: ["switch", "bộ chuyển mạch", "Ethernet switch", "Layer 2 switch"]
Related to:
  - "[[mac-address]]"
  - "[[frame]]"
  - "[[broadcast-domain]]"
  - "[[ip-network]]"
  - "[[router]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Bộ chuyển mạch (switch)

**Bộ chuyển mạch (switch)** là thiết bị **nối các host trong cùng một mạng cục bộ (LAN)** [1].

## Cách nó chuyển dữ liệu

- Bộ chuyển mạch (switch) chuyển **khung dữ liệu (frame)** dựa trên **địa chỉ MAC đích**, và **không đọc địa chỉ IP** [1].
- Trong cơ chế **flood-and-learn — tràn gói tin rồi học địa chỉ**, khi chưa biết MAC đích, bộ chuyển mạch (switch) **chuyển bản sao ARP request ra mọi cổng** trong miền quảng bá (broadcast domain). Sau khi học được vị trí, nó tra bảng MAC của mình và **chuyển khung dữ liệu (frame) chỉ ra cổng nối với đích** [1].

## Ranh giới với bộ định tuyến (router)

Bộ chuyển mạch (switch) gắn với **phạm vi một mạng con (subnet)**: mọi thiết bị nó nối nằm trong cùng một miền quảng bá (broadcast domain) [1]. Muốn nối hai mạng con (subnet) khác nhau phải dùng **[[router]]**. Trong nhiều thiết kế hiện đại, vai trò này được gộp trong một thiết bị **Layer 3 switch** (bộ chuyển mạch (switch) có khả năng định tuyến) [1].

Do chỉ chuyển dữ liệu trong cùng mạng con (subnet), bộ chuyển mạch (switch) **không phải là ranh giới chặn quảng bá (broadcast)** — vai trò đó thuộc về [[router]].

Xem thêm: [[mac-address]], [[frame]], [[broadcast-domain]], [[ip-network]], [[router]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.
