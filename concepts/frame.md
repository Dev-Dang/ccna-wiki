---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["frame", "khung dữ liệu", "Ethernet frame"]
Related to:
  - "[[mac-address]]"
  - "[[switch]]"
  - "[[router]]"
  - "[[ip-address]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Khung dữ liệu (frame)

**Khung dữ liệu (frame)** là **đơn vị dữ liệu ở tầng Ethernet** — tầng liên kết dữ liệu của mạng cục bộ (LAN) [1].

## Cấu trúc cơ bản

- Mỗi khung dữ liệu (frame) mang **địa chỉ MAC nguồn** và **địa chỉ MAC đích**.
- **Gói tin IP nằm bên trong khung dữ liệu (frame)** — IP là "hàng hoá", khung dữ liệu (frame) là "phong bì" của chặng Ethernet [1].

## Vì sao phải phân biệt khung dữ liệu (frame) với gói tin IP

Hai tầng đọc hai thứ khác nhau:

- **Bộ chuyển mạch (switch)** chỉ đọc **MAC đích** trên khung dữ liệu (frame) để chuyển tiếp, không đọc địa chỉ IP.
- **Bộ định tuyến (router)** đọc **địa chỉ IP đích** bên trong gói tin để chọn đường [1].

Hệ quả khi một gói tin đi qua nhiều chặng: **địa chỉ IP đích giữ nguyên**, nhưng **khung dữ liệu (frame)** (và MAC đích của nó) được dựng lại ở mỗi chặng. Vì vậy một khung dữ liệu (frame) chỉ sống trong phạm vi một chặng Ethernet, không đi xuyên suốt đường truyền.

Xem thêm: [[mac-address]], [[switch]], [[router]], [[ip-address]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.
