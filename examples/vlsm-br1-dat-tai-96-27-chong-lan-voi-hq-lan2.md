---
type: Example
domain: networking
source_type: file
date: 2026-10-06
status: Draft
_organized: true
_icon: clipboard-list
Related to:
  - "[[chia-mang-khong-tao-dia-chi-moi-chi-phan-vung-lai-khoi]]"
  - "[[vlsm]]"
  - "[[network-mask]]"
  - "[[chia-subnet-theo-so-host]]"
Belongs to: []
Has: []
lesson: "Chồng lấn (overlap) phát hiện được bằng mắt qua một phép so sánh duy nhất: nếu địa chỉ quảng bá (broadcast) của mạng con trước lớn hơn hoặc bằng Subnet ID của mạng con sau thì hai mạng đó chồng nhau."
sources:
  - id: "vlsm-zero"
    ref: "[1]"
    paragraph: "§Chồng lấn xảy ra như thế nào"
  - id: "vlsm-zero"
    ref: "[2]"
    paragraph: "§Chồng lấn xảy ra như thế nào"
---

# Đặt BRANCH LAN1 tại .96/27 làm nó chồng lấn với HQ LAN2 .64/26

## Bối cảnh

Ta chia `192.168.40.0/24` cho năm mạng: HQ LAN1 (50 host), HQ LAN2 (50 host), BRANCH LAN1 (30 host), BRANCH LAN2 (12 host) và một link HQ–BRANCH (2 host). Mạng con (subnet) BRANCH LAN1 cần khối 32 địa chỉ, tức `/27`.

Đáp án đúng đặt BRANCH LAN1 tại `.128/27`. Case này xét điều gì xảy ra nếu ta **đặt nhầm nó tại `.96/27`** — và vì sao đó là lỗi chứ không phải một lựa chọn khác.

## Tình huống

Khối `/27` bắt đầu tại `.96` phủ từ `.96` đến `96 + 32 − 1 = .127`.

Nhưng HQ LAN2 đã được cấp `.64/26`, phủ từ `.64` đến `.127` trước đó.

Vùng `.96 – .127` (32 địa chỉ) **thuộc cả hai mạng con (subnet) cùng lúc**. Đó là chồng lấn (overlap).

Bản đồ đầy đủ 256 địa chỉ, ký hiệu `XX` là địa chỉ bị hai mạng con cùng giành. Mỗi hàng là 16 địa chỉ; địa chỉ = số đầu hàng + số cột (hàng `.160`, cột `+3` là `.163`). Chữ số là mạng (`1` = HQ LAN1, `2` = HQ LAN2, `4` = BRANCH LAN2, `5` = Link), chữ cái là vai trò (`N` = Subnet ID, `H` = host, `B` = BroadCast), `..` là chưa cấp.

```text
        +0  +1  +2  +3  +4  +5  +6  +7  +8  +9  +10 +11 +12 +13 +14 +15
.0       1N  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H
.16      1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H
.32      1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H
.48      1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1H  1B
.64      2N  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H
.80      2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H  2H
.96      XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX
.112     XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX  XX
.128     ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..
.144     ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..
.160     4N  4H  4H  4H  4H  4H  4H  4H  4H  4H  4H  4H  4H  4H  4H  4B
.176     5N  5H  5H  5B  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..
.192     ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..
.208     ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..
.224     ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..
.240     ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..  ..
```

Hậu quả quan sát được:

- Địa chỉ `.100` **hợp lệ ở cả HQ LAN2 lẫn BRANCH LAN1**, nên hai máy ở hai nơi có thể cùng được gán `.100`.
- Bộ định tuyến (router) có **hai tuyến cùng khớp** `.100`: `.64/26` và `.96/27`. Nó thường chọn tuyến có **prefix dài hơn** (`/27`), nên gói gửi tới máy `.100` của HQ LAN2 có thể bị đưa sang BRANCH và **không bao giờ tới đích**.
- Khi cấu hình trên router Cisco, IOS **từ chối địa chỉ thứ hai** và báo lỗi `"overlaps with"` [2].

Cách phát hiện bằng mắt: **nếu địa chỉ quảng bá (broadcast) của mạng con trước lớn hơn hoặc bằng Subnet ID của mạng con sau thì chúng chồng lấn** [2]. Ở đây broadcast của HQ LAN2 là `.127`, Subnet ID của BRANCH LAN1 là `.96`, và `127 ≥ 96` → chồng lấn.

Đối chiếu với đáp án đúng (BRANCH LAN1 tại `.128/27`), phép kiểm tra này cho `127 < 128` → hợp lệ.

## Giải thích

Case này là biểu hiện cụ thể của [[chia-mang-khong-tao-dia-chi-moi-chi-phan-vung-lai-khoi]]: mỗi địa chỉ phải thuộc **đúng một** mạng con (subnet), và `.96–.127` bị hai mạng cùng nhận là vi phạm điều đó.

Gốc rễ lỗi nằm ở **luật căn lề**: khối kích thước 32 (`2⁵`) chỉ được bắt đầu tại **bội số của 32**, tức `0, 32, 64, 96, 128, 160, 192, 224`. `.96` **thỏa** căn lề, nên lỗi ở đây không phải lỗi ranh giới mà là lỗi **thứ tự cấp phát** — `.96` nằm trong vùng mà HQ LAN2 (khối lớn hơn, được cấp trước) đã chiếm.

Điều này giải thích luật "**lớn trước, nhỏ sau**" của VLSM: cấp khối lớn trước thì mỗi khối sau tự rơi vào đúng ranh giới còn trống, và **phép kiểm tra `broadcast + 1 = Subnet ID sau` tự thỏa**. Cấp khối nhỏ trước là con đường ngắn nhất dẫn tới chồng lấn.

*Nếu đặt nhầm tại `.144/27`: `.144` không chia hết cho 32 nên đó là lỗi khác — Subnet ID lệch ranh giới, bị từ chối ngay cả khi vùng đó còn trống.* [2]

Xem thêm: [[chia-mang-khong-tao-dia-chi-moi-chi-phan-vung-lai-khoi]], [[chia-subnet-theo-so-host]], [[vlsm]]

## Tài liệu tham khảo

[1] NetworkAcademy.io. "VLSM without Binary Example 1." [Online]. Available: https://www.networkacademy.io/ccna/ip-subnetting/vlsm-without-binary-example1

[2] J. Mutai. "VLSM Subnetting Explained: How to Subnet by Host Requirements," ComputingForGeeks, cập nhật 17/06/2026. [Online]. Available: https://computingforgeeks.com/subnetting-vlsm-explained/