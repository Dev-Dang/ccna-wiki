---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["VLSM", "Variable-Length Subnet Masking", "mặt nạ mạng con có độ dài thay đổi"]
Related to:
  - "[[classless-addressing]]"
  - "[[cidr]]"
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

# VLSM (Variable-Length Subnet Masking)

**VLSM (Variable-Length Subnet Masking – mặt nạ mạng con có độ dài thay đổi)** là kỹ thuật cho phép một tổ chức **chia một khối địa chỉ thành các mạng con (subnet) có kích thước khác nhau**, mỗi mạng con (subnet) có **prefix riêng**, sao cho vừa khít nhất với nhu cầu của từng phần trong liên mạng [1][2].

## Vấn đề nó giải quyết

Với mô hình classful, một tổ chức chỉ có ba cỡ khối để chọn (/8, /16, /24). Nếu mỗi cửa hàng chỉ cần 10 host, việc cấp cả một khối lớp C (256 địa chỉ) là lãng phí. VLSM cho phép cắt khối đó thành nhiều phần với cỡ khác nhau — chỗ cần nhiều host thì cấp khối lớn, chỗ cần ít thì cấp khối nhỏ.

## Cách nó hoạt động

Ý tưởng: **kích thước của một mạng IP (IP network) không nhất thiết phải cố định theo mặt nạ mạng (network mask) của từng lớp**. Một khối `/24` có thể được cắt thành:

- một mạng con (subnet) `/26` (64 địa chỉ) cho phần cần nhiều host,
- hai mạng con (subnet) `/27` (32 địa chỉ) cho phần cần ít host hơn,
- phần còn lại (`/25`) để dành cho tăng trưởng.

Mỗi mạng con (subnet) như vậy có prefix riêng, và mặt nạ mạng (network mask) phải được khai báo tường minh — đây là hệ quả trực tiếp của việc chuyển sang [[classless-addressing]].

## Ràng buộc khi dùng VLSM

✦ *Heuristic* [1]:

- **Cấp khối lớn nhất trước**, các khối nhỏ sau.
- Mỗi khối kích thước `2ᵏ` phải **bắt đầu tại bội số của `2ᵏ`** (căn lề khối). Ví dụ khối 64 địa chỉ phải bắt đầu tại các địa chỉ chia hết cho 64.

Việc cấp phát thủ công dễ gây **chồng lấn (overlap)** nếu không theo đúng ràng buộc căn lề — xem quy trình chi tiết tại [[chia-subnet-theo-so-host]].

Xem thêm: [[classless-addressing]], [[cidr]], [[chia-subnet-theo-so-host]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.