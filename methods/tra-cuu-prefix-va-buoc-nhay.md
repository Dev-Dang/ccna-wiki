---
type: Method
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: wrench
aliases: ["tra cứu prefix và bước nhảy", "prefix lookup", "bước nhảy subnet", "magic number subnetting"]
applies_when: "Cần tra nhanh số host của một prefix (/24, /26, /30...) hoặc tính ranh giới các subnet khi bước nhảy không rơi vào ranh giới octet."
Related to:
  - "[[network-mask]]"
  - "[[classless-addressing]]"
  - "[[cidr]]"
  - "[[vlsm]]"
  - "[[chia-subnet-theo-so-host]]"
Belongs to: []
Has:
  - "Bảng tra nhanh prefix /24–/30"
  - "Phương pháp bước nhảy (256 − octet mask)"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "vlsm-zero"
    ref: "[2]"
    paragraph: "§Nền tảng cần nắm trước khi tính"
---
# Tra cứu prefix và bước nhảy (prefix lookup & jump)

## Điều kiện tiên quyết

- Phải biết **mặt nạ mạng (network mask)** hoặc **prefix** của dải cần tra.
- Phải hiểu phép AND bit với mask để lấy mạng — xem [[network-mask]].

## Các bước

### 1. Tra bảng nhanh cho prefix phổ biến

| Prefix | Mask | Số host dùng được | Ghi chú |
| --- | --- | --- | --- |
| /24 | 255.255.255.0 | 254 | 1 lớp C nguyên |
| /25 | 255.255.255.128 | 126 | cắt đôi /24 |
| /26 | 255.255.255.192 | 62 | 4 subnet |
| /27 | 255.255.255.224 | 30 | 8 subnet |
| /28 | 255.255.255.240 | 14 | 16 subnet |
| /29 | 255.255.255.248 | 6 | 32 subnet |
| /30 | 255.255.255.252 | 2 | link point-to-point (điểm-điểm) |

### 1b. Tính ngược từ số host ra prefix (không cần bảng)

Khi biết số host cần nhưng không muốn tra bảng, dùng công thức [2]:

```text
prefix = 32 − ⌈log₂(host + 2)⌉
```

`⌈ ⌉` là làm tròn lên (`⌈5,7⌉ = 6`). Số `+ 2` là hai địa chỉ bị giữ lại: địa chỉ mạng (Subnet ID) và địa chỉ quảng bá (broadcast) [2].

| Host cần | host + 2 | log₂ | Làm tròn lên | n (bit host) | Prefix | Block size |
| --- | --- | --- | --- | --- | --- | --- |
| 50 | 52 | 5,70 | 6 | 6 | /26 | 64 |
| 30 | 32 | 5,00 | 5 | 5 | /27 | 32 |
| 12 | 14 | 3,81 | 4 | 4 | /28 | 16 |
| 2 | 4 | 2,00 | 2 | 2 | /30 | 4 |

✦ *Heuristic:* dãy số host dùng được `2 – 6 – 14 – 30 – 62 – 126` chính là `2ⁿ − 2`. Chỉ cần nhớ dãy này là đủ để chọn prefix bằng mắt [2].

Công thức này tương đương bước "chọn prefix `/n` nhỏ nhất sao cho `2^(32−n) ≥ host + 2`" trong [[chia-subnet-theo-so-host]] — chỉ khác cách viết.

### 2. Tính bước nhảy khi mask không rơi vào ranh giới octet

**Bước nhảy = 256 − octet mask đang thay đổi.** Ví dụ `255.255.224.0` → octet thứ 3 = 224 → bước nhảy = `256 − 224 = 32` [1].

Với ví dụ `10.1.1.2/19` (mask `255.255.224.0`) [1]:

- Bước nhảy ở octet thứ 3 = **32** → các subnet là `10.1.0.0/19`, `10.1.32.0/19`, `10.1.64.0/19`…
- Áp AND: `1.1` với octet thứ 3 = 1 → `1` rơi vào dải `0–31` → mạng = `10.1.0.0/19`.
- Số host = `2^13 − 2 = 8.190` (13 bit host = 19 prefix trừ 6 bit của 2 octet đầu).

### 3. Áp dụng với ví dụ dễ hơn: octet mask rơi vào ranh giới

Với `10.4.21.43/255.0.0.0` [1]: mask chỉ ảnh hưởng octet đầu → mạng = `10.0.0.0/8`. 
Octet 2–4 giữ nguyên vì mask ở đó toàn `0`.

## Kết quả / Tiêu chí hoàn tất

- Chỉ ra đúng **mạng con (subnet)** từ một địa chỉ bất kỳ + mask.
- Tra được **số host dùng được** của một prefix mà không cần tính tay từng bước.
- Xác định được **bước nhảy giữa các subnet** khi octet mask không phải `0` hoặc `255`.

## Khi nào không nên dùng

- Khi cần chia subnet **theo số host cụ thể cho nhiều nhóm khác nhau** → dùng [[chia-subnet-theo-so-host]] (VLSM) thay vì tra bảng.
- Khi cần tính subnet nhanh với prefix "tròn" (/24, /25) → bảng ở Bước 1 đủ, không cần bước nhảy.

## Luyện tập

- `192.168.1.100/26` → mạng nào? (gợi ý: bước nhảy 64)
- `172.16.5.30/19` → mạng nào?

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] NetworkAcademy.io. "VLSM without Binary Example 1." [Online]. Available: https://www.networkacademy.io/ccna/ip-subnetting/vlsm-without-binary-example1
