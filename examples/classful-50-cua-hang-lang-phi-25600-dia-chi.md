---
type: Example
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: clipboard-list
Related to:
  - "[[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]]"
  - "[[classful-addressing]]"
  - "[[vlsm]]"
Belongs to: []
Has: []
lesson: "Chọn cỡ khối theo mô hình classful là ba cỡ thô có thể gây lãng phí địa chỉ hàng chục lần, và đó là động lực trực tiếp dẫn đến classless/VLSM."
sources:
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[1]"
  - id: "ip-addressing-learn"
    ref: "[2]"
---

# Classful khiến công ty 50 cửa hàng tốn 25.600 địa chỉ cho nhu cầu 500 host

## Bối cảnh

Một công ty bán lẻ có **50 cửa hàng nhỏ** ở các thành phố khác nhau trên khắp nước Mỹ. Mỗi cửa hàng chỉ có **10 host**. Công ty phải kết nối các cửa hàng về trung tâm, nên cần thêm **một mạng cho mỗi đường truyền WAN (Wide Area Network – mạng diện rộng)** tới từng cửa hàng.

Câu hỏi: theo mô hình [[classful-addressing]], công ty cần bao nhiêu địa chỉ?

## Tình huống

Theo classful, mỗi mạng phải là một khối lớp C (`/24`) với 256 địa chỉ.

- Số mạng cần: **50 cửa hàng + 50 đường WAN = hơn 100 mạng lớp C**.
- Tổng địa chỉ tiêu tốn: `100 × 256 = ` **25.600 địa chỉ**.
- Nhu cầu host thực tế: `50 × 10 = ` **500 host**.

| Khoản | Số lượng |
|---|---|
| Cửa hàng | 50 |
| Host mỗi cửa hàng | 10 |
| Tổng host cần | 500 |
| Mạng lớp C phải cấp | >100 |
| Địa chỉ tiêu tốn | 25.600 |
| Địa chỉ thực sự cần (nếu cấp đúng cỡ) | ~1.000 |

Kết quả: chỉ để phục vụ 500 host, công ty chiếm **25.600 địa chỉ** — lãng phí khoảng **50 lần**.

## Giải thích

Hai chiều của cái giá:

- **Lãng phí địa chỉ:** mỗi cửa hàng dùng 10 địa chỉ nhưng chiếm 256; mỗi đường WAN chỉ có **2 đầu** nhưng cũng chiếm trọn một khối lớp C.
- **Áp lực bảng định tuyến (routing table):** cấp hơn 100 khối lớp C rời rạc cho một tổ chức làm bảng định tuyến (routing table) toàn cầu phình ra — đây là một động lực trực tiếp của [[cidr]] [1][2].

So sánh: nếu được chọn cỡ khối theo [[vlsm]] [2]:

- Mỗi cửa hàng cần `10 + 2 = 12` địa chỉ → chọn `/28` (16 địa chỉ). 50 cửa hàng tốn **800 địa chỉ**.
- Mỗi link WAN dùng `/30` (4 địa chỉ) → tốn thêm **200**. Tổng khoảng **1.000 địa chỉ** (bằng ~1/25).
- Nếu dùng `/31` cho link WAN (xem [[prefix-31-tiet-kiem-dia-chi-cho-link-diem-diem]]), tổng còn khoảng **900**.

*Cách tính tổng `1.000` và `900` lấy từ bảng phân bổ classless trong tài liệu [2]: 50 × 16 (cửa hàng, /28) + 50 × 4 (WAN, /30) = 1.000; thay `/30` bằng `/31` (2 địa chỉ/link) thì `800 + 100 = 900`.*

→ Cùng một nhu cầu, cách cấp khối quyết định chênh lệch hàng chục lần về địa chỉ tiêu tốn. Đây chính là bài toán mà [[classless-addressing]] sinh ra để giải.

Xem thêm: [[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]], [[chia-subnet-theo-so-host]]

## Tài liệu tham khảo

[1] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.

[2] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.