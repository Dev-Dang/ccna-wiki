---
type: Insight
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: lightbulb
claim: "Mô hình classful lãng phí địa chỉ vì chỉ có ba cỡ khối quá thô để vừa khít nhu cầu"
confidence: high
Related to:
  - "[[classful-addressing]]"
  - "[[classless-addressing]]"
Has:
  - "[[classful-50-cua-hang-lang-phi-25600-dia-chi]]"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Mô hình classful lãng phí địa chỉ vì chỉ có ba cỡ khối quá thô để vừa khít nhu cầu

## Bối cảnh

Mô hình **[[classful-addressing]]** chỉ cho phép chọn giữa ba cỡ khối: `/8` (lớp A), `/16` (lớp B), `/24` (lớp C). Nhu cầu thực tế của các tổ chức thì rất đa dạng — nhiều nơi chỉ cần vài chục host.

## Luận điểm

Khi nhu cầu rơi vào khoảng giữa hai cỡ khối, tổ chức buộc phải chọn cỡ **lớn hơn** và **lãng phí phần dư**. Cái giá thể hiện ở **hai chiều** [1]:

1. **Lãng phí địa chỉ.** Mỗi cửa hàng chỉ cần 10 host nhưng phải chiếm cả một khối lớp C (256 địa chỉ); một đường WAN chỉ có 2 đầu cũng chiếm trọn một khối.
2. **Áp lực lên bảng định tuyến (routing table).** Cấp nhiều khối lớp C rời rạc cho một tổ chức làm bảng định tuyến (routing table) toàn cầu phình ra. Đây là một **động lực trực tiếp** của [[cidr]] [1].

## Bằng chứng định lượng

✦ *Tính toán minh hoạ từ nguồn:* một công ty có **50 cửa hàng, mỗi cửa hàng 10 host**, cộng một mạng cho mỗi đường truyền WAN. Theo classful:

- Số mạng cần: **hơn 100** (50 cửa hàng + 50 đường WAN).
- Tổng địa chỉ tiêu tốn: >100 khối lớp C × 256 = khoảng **25.600 địa chỉ**.
- Nhu cầu thực tế: chỉ khoảng **500 host**.

→ Tỉ lệ lãng phí khoảng **50 lần**. Xem case đầy đủ tại [[classful-50-cua-hang-lang-phi-25600-dia-chi]].

Ngược lại, nếu được chọn đúng cỡ khối như trong [[classless-addressing]], cùng nhu cầu đó chỉ tốn khoảng **1.000 địa chỉ** — bằng khoảng **1/25** [1].

## Phạm vi / ngoại lệ

Vấn đề **không nằm ở cơ chế** của classful (mặt nạ mạng (network mask) suy ra từ lớp là tiện lợi) mà ở **độ thô của ba cỡ khối**. Đây là lý do ra đời của [[vlsm]] và [[classless-addressing]].

Xem thêm: [[classful-addressing]], [[classless-addressing]], [[classful-50-cua-hang-lang-phi-25600-dia-chi]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.