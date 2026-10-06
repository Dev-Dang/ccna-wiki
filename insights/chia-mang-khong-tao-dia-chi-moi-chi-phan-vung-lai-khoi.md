---
type: Insight
domain: networking
source_type: file
date: 2026-10-06
status: Draft
_organized: false
_icon: lightbulb
claim: "Chia một khối địa chỉ thành nhiều mạng con (subnet) không tạo thêm địa chỉ — nó chỉ phân vùng lại khối cũ, và mỗi địa chỉ phải thuộc đúng một mạng con hoặc còn trống"
confidence: high
Related to:
  - "[[vlsm]]"
  - "[[subnet-cung-mot-khoi-van-phai-qua-router]]"
  - "[[ip-address]]"
Belongs to: []
Has:
  - "[[vlsm-br1-dat-tai-96-27-chong-lan-voi-hq-lan2]]"
sources:
  - id: "vlsm-zero"
    ref: "[1]"
    paragraph: "§Nhìn toàn bộ 256 địa chỉ: chia xong vẫn là một khối"
  - id: "vlsm-zero"
    ref: "[2]"
    paragraph: "§Nhìn toàn bộ 256 địa chỉ: chia xong vẫn là một khối"
---

# Chia một khối thành nhiều mạng con (subnet) không tạo thêm địa chỉ — nó chỉ phân vùng lại khối cũ

## Bối cảnh

Có một hiểu nhầm rất tự nhiên khi mới học chia mạng con (subnetting): nếu một khối `/24` có 256 địa chỉ, chia nó thành nhiều mạng con (subnet) thì nghe như mạng sẽ "to ra" hoặc có thêm địa chỉ để dùng. Thực tế ngược lại — và hiểu sai điểm này dẫn thẳng tới lỗi chồng lấn (overlap).

## Luận điểm

**Chia mạng không tạo thêm địa chỉ.** Khối `192.168.40.0/24` luôn có đúng 256 địa chỉ (từ `.0` đến `.255`), trước và sau khi chia [1][2].

Chia nghĩa là **phân vùng**: mỗi địa chỉ thuộc **đúng một** mạng con (subnet), hoặc còn trống. Giống chia 256 ô đất thành các lô — mỗi ô chỉ nằm trong một lô, không ô nào thuộc hai lô cùng lúc.

Hệ quả trực tiếp:

- Mục đích của chia mạng **không phải** "làm mạng to hơn", mà là **tách thành nhiều mạng riêng** — mỗi LAN một dải — và **không bỏ phí địa chỉ** [1][2].
- Nếu hai mạng con (subnet) cùng chứa một địa chỉ thì đó chính là **chồng lấn (overlap)**, và là lỗi cấu hình.

## Cơ chế

Tổng số địa chỉ được bảo toàn vì mọi mạng con (subnet) đều là **tập con rời nhau** của khối gốc. Cộng kích thước các mạng con với phần còn trống luôn ra đúng kích thước khối gốc.

Ví dụ với `192.168.40.0/24` chia cho 4 mạng con (subnet) và một link:

| Vùng | Địa chỉ | Số địa chỉ |
|---|---|---|
| Mạng con (subnet) 1 | `.0 – .63` | 64 |
| Mạng con (subnet) 2 | `.64 – .127` | 64 |
| Mạng con (subnet) 3 | `.128 – .159` | 32 |
| Mạng con (subnet) 4 | `.160 – .175` | 16 |
| Link | `.176 – .179` | 4 |
| Chưa cấp | `.180 – .255` | 76 |
| **Tổng** | `.0 – .255` | **256** |

`64 + 64 + 32 + 16 + 4 + 76 = 256` — không địa chỉ nào bị đếm hai lần [1].

## Phạm vi / ngoại lệ

- Hai mạng **hoàn toàn tách biệt** (hai công ty khác nhau, hoặc nối với nhau qua NAT — Network Address Translation, tức đổi địa chỉ khi gói đi qua ranh giới) vẫn có thể dùng **cùng một dải địa chỉ**, vì chúng không nằm trong cùng một miền định tuyến [1].
- Nhưng trong **một mạng nội bộ được định tuyến chung**, mỗi địa chỉ phải thuộc đúng một mạng con (subnet). Đây là lý do chồng lấn (overlap) là lỗi chứ không phải một lựa chọn hợp lệ.

Xem thêm: [[vlsm]], [[subnet-cung-mot-khoi-van-phai-qua-router]], [[vlsm-br1-dat-tai-96-27-chong-lan-voi-hq-lan2]]

## Tài liệu tham khảo

[1] NetworkAcademy.io. "What is VLSM?" [Online]. Available: https://www.networkacademy.io/ccna/ip-subnetting/what-is-vlsm

[2] NetworkAcademy.io. "VLSM without Binary Example 1." [Online]. Available: https://www.networkacademy.io/ccna/ip-subnetting/vlsm-without-binary-example1