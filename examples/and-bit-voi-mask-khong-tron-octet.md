---
type: Example
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: true
_icon: clipboard-list
Related to:
  - "[[network-mask]]"
  - "[[ip-network]]"
  - "[[host]]"
  - "[[tra-cuu-prefix-va-buoc-nhay]]"
Belongs to: []
Has: []
lesson: "Phép AND bit với mặt nạ mạng (network mask) là cơ chế quyết định một địa chỉ IP thuộc mạng nào; khi mask không rơi ranh giới octet, bước nhảy = 256 − octet mask là cách tính nhanh ranh giới các subnet."
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---
# AND bit với mask không tròn octet: 10.1.1.2/19 ra 10.1.0.0/19

## Bối cảnh

Máy tính có địa chỉ `10.1.1.2` với mặt nạ mạng (network mask) `255.255.224.0`. Đây là mask **không rơi vào ranh giới octet** (octet thứ 3 = 224, không phải 0 hay 255), khiến việc "nhìn bằng mắt" tính mạng thường gây nhầm.

Phép AND bit là cơ chế [[host]] dùng để tự quyết định một đích là cùng mạng hay khác mạng.

## Tình huống

### Bước 1 — Xác định octet thay đổi và bước nhảy

Mask `255.255.224.0` ảnh hưởng octet thứ 3 (`224`). **Bước nhảy = 256 − 224 = 32.** Vậy các mạng con (subnet) là: `10.1.0.0/19`, `10.1.32.0/19`, `10.1.64.0/19`, … [1]

### Bước 2 — AND bit octet thứ 3

|  | Giá trị | Nhị phân |
| --- | --- | --- |
| Octet thứ 3 của host | 1 | `00000001` |
| Octet thứ 3 của mask | 224 | `11100000` |
| AND | 0 | `00000000` |

→ Octet thứ 3 cho ra `0`, rơi vào dải `0–31` → **mạng =** `10.1.0.0/19` [1].

### Bước 3 — Đếm host

Prefix `/19` → 19 bit mạng, 13 bit host → số host dùng được = `2^13 − 2 = 8.190` [1].

### Ví dụ đối chiếu: mask rơi vào ranh giới octet

Với `10.4.21.43` và mask `255.0.0.0`, mask chỉ ảnh hưởng octet đầu → mạng = `10.0.0.0/8`; octet 2–4 giữ nguyên vì mask toàn `0` [1].

## Giải thích

Ba điều rút ra:

1. **Mask quyết định toàn bộ khái niệm "mạng".** Địa chỉ `10.1.1.2` có thể thuộc `10.1.0.0/19`, `10.0.0.0/8`, hay `10.1.1.0/24` — tất cả phụ thuộc vào mask được gán [1].
2. **Bước nhảy là công thức ngắn để tìm ranh giới subnet** khi octet mask không phải 0/255: `bước nhảy = 256 − octet mask` [1]. Từ đó suy ra các dải `0–31`, `32–63`, … mà không cần AND từng bit.
3. **AND bit là cơ chế thật, không chỉ là mẹo tính tay:** chính nó cho phép [[host]] biết đích cùng mạng (gửi trực tiếp qua [[switch]]) hay khác mạng (gửi cho cổng mặc định (default gateway)) [1].

Xem thêm: [[network-mask]], [[ip-network]], [[host]], [[tra-cuu-prefix-va-buoc-nhay]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.
