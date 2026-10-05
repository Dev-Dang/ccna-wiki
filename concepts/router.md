---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["router", "bộ định tuyến"]
Related to:
  - "[[ip-network]]"
  - "[[broadcast-domain]]"
  - "[[default-gateway]]"
  - "[[network-address-translation]]"
  - "[[subnet-cung-mot-khoi-van-phai-qua-router]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Bộ định tuyến (router)

**Bộ định tuyến (router)** là thiết bị **nối các mạng IP khác nhau**: nó đọc **địa chỉ IP đích** và **tra bảng định tuyến (routing table)** để chọn đường chuyển tiếp [1].

## Vai trò làm ranh giới

Bộ định tuyến (router) là **điểm cắt miền quảng bá (broadcast domain)**: nó **không chuyển tiếp gói quảng bá (broadcast)**, thay vào đó chọn đường theo bảng định tuyến (routing table). Nhờ vậy nó **chấm dứt cơ chế flood-and-learn — tràn gói tin rồi học địa chỉ** giữa các mạng [1].

Đây cũng là **ranh giới giữa các mạng con (subnet)**: hai host **khác mạng con (subnet)** bắt buộc giao tiếp qua một hoặc nhiều bộ định tuyến (router), **kể cả khi hai mạng con (subnet) đó được chia ra từ cùng một khối địa chỉ** [1]. Xem chi tiết tại [[subnet-cung-mot-khoi-van-phai-qua-router]].

## Bộ định tuyến (router) nhìn thấy gì khi chuyển tiếp

- **Địa chỉ IP đích giữ nguyên** suốt đường đi.
- **Địa chỉ MAC đích được thay ở mỗi chặng** — MAC chỉ có ý nghĩa trong từng chặng (hop) Ethernet [1].

## Hai vai trò khác thường gắn với bộ định tuyến (router)

- Interface của bộ định tuyến (router) nằm trong cùng mạng con (subnet) với host đóng vai trò **[[default-gateway]]** cho host đó.
- Ở biên mạng, bộ định tuyến (router) là nơi thường chạy **[[network-address-translation]]**. Hai vai trò "cổng mặc định (default gateway)" và "thiết bị làm NAT" **chỉ trùng nhau khi cùng một thiết bị đảm nhận cả hai** [1].

Xem thêm: [[ip-network]], [[broadcast-domain]], [[default-gateway]], [[subnet-cung-mot-khoi-van-phai-qua-router]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.
