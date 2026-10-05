---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["private address", "địa chỉ private", "RFC 1918", "địa chỉ riêng", "shared address space"]
Related to:
  - "[[network-address-translation]]"
  - "[[classful-addressing]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Địa chỉ private (private IP address)

**Địa chỉ private (private IP address)** là các khối địa chỉ IP được dành riêng cho **mạng nội bộ** và **không định tuyến được trên Internet công cộng**. Chúng giải quyết vấn đề cạn kiệt địa chỉ: nhiều tổ chức (và hàng triệu thiết bị) có thể dùng **cùng một dải private** mà không xung đột, vì chúng chỉ có ý nghĩa trong phạm vi mạng nội bộ.

## Ba khối private của RFC 1918

| Khối | Dải địa chỉ | Ghi chú |
|---|---|---|
| `10.0.0.0/8` | `10.0.0.0` – `10.255.255.255` | Một khối lớp A nguyên |
| `172.16.0.0/12` | `172.16.0.0` – `172.31.255.255` | 16 khối lớp B liền kề |
| `192.168.0.0/16` | `192.168.0.0` – `192.168.255.255` | 256 khối lớp C liền kề |

Ba khối này không định tuyến được trên Internet công cộng [1].

## Khối shared address space (RFC 6598)

Ngoài RFC 1918, khối **`100.64.0.0/10`** (RFC 6598) là **shared address space** — cũng không định tuyến toàn cầu, và được dùng cho **Carrier-Grade NAT** (xem [[network-address-translation]]).

## Vì sao private quan trọng

Kết hợp với **[[network-address-translation]]** (NAT), nhiều thiết bị có thể dùng chung **một địa chỉ public** khi ra Internet. Đây là một trong ba cơ chế chính giúp kéo dài tuổi thọ IPv4 [1].

Xem thêm: [[network-address-translation]], [[ip-address]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.