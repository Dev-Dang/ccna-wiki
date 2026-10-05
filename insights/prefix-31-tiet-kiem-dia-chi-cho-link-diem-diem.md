---
type: Insight
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: lightbulb
claim: "Prefix /31 cho phép dùng cả hai địa chỉ cho host trên link điểm-điểm (point-to-point), tiết kiệm địa chỉ so với /30"
confidence: medium
Related to:
  - "[[network-mask]]"
  - "[[chia-subnet-theo-so-host]]"
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Prefix /31 cho phép dùng cả hai địa chỉ cho host trên link điểm-điểm (point-to-point), tiết kiệm địa chỉ so với /30

## Bối cảnh

Công thức phổ biến để tính số host khả dụng trong một mạng con (subnet) là **`2ⁿ − 2`** (với `n` là số bit host), vì hai địa chỉ bị giữ lại làm **địa chỉ mạng** (toàn bit 0) và **địa chỉ quảng bá** (toàn bit 1). Áp công thức này cho prefix `/31` (chỉ có 1 bit host, `2¹ = 2` địa chỉ) sẽ ra **0 host** — nghe vô nghĩa.

## Luận điểm

**RFC 3021** cho phép **cả hai địa chỉ của một prefix `/31` được dùng làm địa chỉ host** trên **link điểm-điểm (point-to-point)** — nơi giao tiếp chỉ có đúng hai đầu và **không cần địa chỉ quảng bá** [1].

Cụ thể:

- **`/31`** → hai địa chỉ, cả hai dùng được cho host → tiết kiệm 2 địa chỉ so với `/30` (vốn có 4 địa chỉ nhưng chỉ 2 dùng được).
- **`/32`** → một địa chỉ duy nhất. ✦ *Heuristic:* dùng để định danh **một host hay một interface cụ thể**, chẳng hạn địa chỉ loopback [1].

## Cơ chế

Địa chỉ mạng và địa chỉ quảng bá bị loại khỏi pool host vì mục đích: định danh mạng và gửi quảng bá. Trên link điểm-điểm **không có host thứ ba** và **không cần quảng bá**, nên cả hai lý do biến mất — kéo theo ràng buộc "mất 2 địa chỉ" cũng không còn cần thiết.

## Phạm vi / ngoại lệ

- **Thiết bị phải hỗ trợ** RFC 3021 (ví dụ Linux, Cisco IOS 12.2 trở lên). Nếu thiết bị không hỗ trợ, `/31` không dùng được theo cách này [1].
- Chỉ áp dụng cho **link điểm-điểm (point-to-point)**, không dùng cho segment nhiều host.
- Đây là **trường hợp biên** của việc tính prefix; với `/n` khi `n ≤ 30`, công thức `2^(32−n) − 2` vẫn đúng.

Xem thêm: [[network-mask]], [[chia-subnet-theo-so-host]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.