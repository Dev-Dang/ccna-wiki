---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: true
_icon: box
aliases: ["classful", "classful addressing", "địa chỉ có lớp", "mô hình classful"]
Related to:
  - "[[classless-addressing]]"
  - "[[network-mask]]"
  - "[[ip-address]]"
Belongs to: []
Has:
  - "[[private-ip-address]]"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Địa chỉ có lớp (classful addressing)

**Địa chỉ có lớp (classful addressing)** là mô hình cấp phát địa chỉ IP xuất hiện trong đặc tả IP năm 1981 (**RFC 791**), chia toàn bộ không gian địa chỉ IPv4 thành **năm lớp: A, B, C, D, E**. Đặc điểm cốt lõi: **lớp quyết định mặt nạ mạng (network mask)**, nên chỉ nhìn địa chỉ là suy ra được mặt nạ mạng (network mask) — không cần khai báo.

## Năm lớp và mặt nạ mạng (network mask) cố định

| Lớp | Bit đầu | Mặt nạ mạng (network mask) | Dải địa chỉ | Số mạng (dùng được) | Host khả dụng/mạng |
|---|---|---|---|---|---|
| **A** | `0` | `/8` | `1.0.0.0` – `126.255.255.255` | 126 | 16.777.214 |
| **B** | `10` | `/16` | `128.0.0.0` – `191.255.255.255` | 16.384 | 65.534 |
| **C** | `110` | `/24` | `192.0.0.0` – `223.255.255.255` | 2.097.152 | 254 |
| **D** | `1110` | — | `224.0.0.0` – `239.255.255.255` | Multicast | — |
| **E** | `1111` | — | `240.0.0.0` – `255.255.255.255` | Dự trữ | — |

Chỉ **ba lớp đầu (A, B, C)** dùng được để gán cho host; lớp D dành cho đa hướng (multicast) và lớp E dự trữ [2].

## Vì sao con số lệch so với bảng "địa chỉ mỗi mạng"

Nhiều tài liệu ghi lớp A có 128 mạng và mỗi mạng có 16.777.216 **địa chỉ**. Hai con số đó là **tổng địa chỉ**, không phải số dùng được. Lý do lệch [1]:

- **0.0.0.0/8** và **127.0.0.0/8** thuộc dải lớp A nhưng bị **dự trữ**. `127.0.0.0/8` dành cho loopback — thuật ngữ giữ nguyên tiếng Anh, nghĩa là địa chỉ để thiết bị tự gọi chính nó. Do đó lớp A chỉ còn **126** mạng dùng được thay vì 128.
- Trong mỗi mạng, **hai địa chỉ không gán cho host**: địa chỉ mạng (network address – toàn bit 0 ở phần host) và địa chỉ quảng bá (broadcast address – toàn bit 1 ở phần host). Nên số host khả dụng là **2ⁿ − 2**.

Với lớp C (`/24`, 8 bit host): `2⁸ − 2 = 254` host khả dụng. Với lớp B (`/16`): `2¹⁶ − 2 = 65.534`.

## Địa chỉ dành riêng bên trong các lớp

Một số khối bị tách ra cho mục đích riêng (xem [[private-ip-address]]):

- Ba khối **private** của RFC 1918: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16` — không định tuyến được trên Internet công cộng.
- Khối **`100.64.0.0/10`** (RFC 6598) là shared address space, cũng không định tuyến toàn cầu.

## Giới hạn của classful

Vấn đề của classful **không nằm ở cơ chế mà ở độ thô của ba cỡ mạng** — chỉ có ba kích thước khối để chọn (/8, /16, /24), trong khi nhu cầu thực tế rất đa dạng. Điều này dẫn đến lãng phí và áp lực lên bảng định tuyến (routing table) (xem [[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]]).

Xem thêm: [[classless-addressing]], [[network-mask]], [[private-ip-address]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.