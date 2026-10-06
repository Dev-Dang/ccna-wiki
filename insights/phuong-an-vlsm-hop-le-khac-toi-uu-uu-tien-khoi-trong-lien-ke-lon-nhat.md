---
type: Insight
domain: networking
source_type: file
date: 2026-10-06
status: Draft
_organized: false
_icon: lightbulb
claim: "Một lời giải VLSM chỉ cần hợp lệ theo bốn luật bất biến, nhưng nên ưu tiên phương án để lại khối trống liền kề lớn nhất — vì 'hợp lệ' khác 'tối ưu'"
confidence: high
Related to:
  - "[[vlsm]]"
  - "[[chia-subnet-theo-so-host]]"
  - "[[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]]"
  - "[[chia-mang-khong-tao-dia-chi-moi-chi-phan-vung-lai-khoi]]"
Belongs to: []
Has: []
sources:
  - id: "vlsm-zero"
    ref: "[1]"
    paragraph: "§Các phương án đặt khác nhau cho cùng bộ khối"
  - id: "vlsm-zero"
    ref: "[2]"
    paragraph: "§Các phương án đặt khác nhau cho cùng bộ khối"
  - id: "vlsm-zero"
    ref: "[3]"
    paragraph: "§Kiểm chứng đáp án và lỗi hay gặp"
---

# Một lời giải VLSM chỉ cần hợp lệ theo bốn luật, nhưng nên ưu tiên phương án để lại khối trống liền kề lớn nhất

## Bối cảnh

Đề bài VLSM (Variable-Length Subnet Masking – mặt nạ mạng con có độ dài thay đổi) chỉ ràng buộc **bốn luật**, không ràng buộc mạng con (subnet) phải bắt đầu ở đâu. Vì vậy cùng một bộ khối có **rất nhiều cách đặt** địa chỉ mạng con (Subnet ID) khác nhau, và tất cả đều được tính là "đúng".

Nhưng các cách đặt đó **không tương đương nhau** khi nhìn về tương lai: có cách để lại một khối trống lớn nguyên vẹn, có cách làm khối trống vụn thành nhiều mảnh nhỏ không dùng được cho việc gì.

## Luận điểm

**"Hợp lệ" khác "tối ưu".** Mọi phương án thỏa bốn luật đều qua bài, nhưng nên chọn phương án **giữ lại khối trống liền kề lớn nhất**.

Tiêu chí đánh giá phương án: **khối trống liền kề lớn nhất càng lớn càng tốt** [1]. Đây chính là cách đo mức **phân mảnh (fragmentation)** — phân mảnh là tình trạng không gian địa chỉ còn trống bị chặt thành nhiều mảnh nhỏ rời rạc, không mảnh nào đủ lớn để cấp cho một mạng mới.

## Cơ chế

Bảng so sánh các phương án cho cùng bộ khối (chia `192.168.40.0/24` cho HQ LAN1 = 50 host, HQ LAN2 = 50 host, BRANCH LAN1 = 30 host, BRANCH LAN2 = 12 host, link = 2 host):

| # | Phương án | Hợp lệ? | Khối trống liền kề lớn nhất |
|---|---|---|---|
| 1 | Chuẩn: LAN1 `.0`, LAN2 `.64`, BR1 `.128`, BR2 `.160`, Link `.176` | ✓ | `/26` (`.192`) |
| 2 | Đổi chỗ hai LAN của HQ | ✓ | `/26` (`.192`) |
| 3 | Đặt link ở cuối (`.252/30`) | ✓ | `/27` |
| 4 | Cấp từ cuối dải xuống | ✓ | `/26` (`.0`) |
| 5 | Nhóm theo site: BRANCH ở đầu, HQ ở sau | ✓ | `/26` (`.192`) |
| 6 | Link đặt đầu tiên (`.0/30`), rồi các LAN | ✓ | `/27` |
| 7 | BR1 đặt tại `.144/27` | ✗ | — (.144 không chia hết 32) |
| 8 | BR1 đặt tại `.96/27` | ✗ | — (chồng lấn với HQ LAN2) |

Đọc bảng này ra ba điều:

- Phương án 3 và 6 **hợp lệ** nhưng làm phần dư **rời rạc** — ví dụ phương án 6 kẹt 60 địa chỉ ở `.4–.63` mà không cấp được cho mạng nào vì khối bắt đầu sai ranh giới [1].
- Phương án 1, 4, 5 giữ nguyên một **khối `/26` liền mạch**, sẵn sàng cho một mạng 62 host sau này.
- Phương án 2 vô hại vì hai LAN của HQ **cùng cỡ** — hoán đổi hai khối bằng nhau không đổi cấu trúc phần dư.

**Góc nhìn định tuyến:** ở phương án chuẩn, hai LAN của HQ (`.0/26` và `.64/26`) **gộp tuyến (route aggregation)** thành một tuyến `192.168.40.0/25`, và ba mạng BRANCH gộp thành `192.168.40.128/26` (`.128–.191`). Bảng định tuyến (routing table) ngắn hơn — xem [[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]] [1].

## Phạm vi / ngoại lệ

- Tiêu chí "khối trống liền kề lớn nhất" là **heuristic thiết kế**, không phải luật bắt buộc. Nếu biết chắc sẽ không cấp thêm mạng nào, phương án để lại nhiều mảnh nhỏ vẫn chạy đúng.
- Source gốc cảnh báo VLSM **đòi hỏi lập kế hoạch cẩn thận hơn** cách chia đều, vì chia ẩu sẽ gây phân mảnh (fragmentation) [1] — phân mảnh ở đây đúng nghĩa tiêu chí đo lường ở trên.

Xem thêm: [[chia-subnet-theo-so-host]], [[chia-mang-khong-tao-dia-chi-moi-chi-phan-vung-lai-khoi]], [[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]]

## Tài liệu tham khảo

[1] NetworkAcademy.io. "What is VLSM?" [Online]. Available: https://www.networkacademy.io/ccna/ip-subnetting/what-is-vlsm

[2] NetworkAcademy.io. "VLSM without Binary Example 1." [Online]. Available: https://www.networkacademy.io/ccna/ip-subnetting/vlsm-without-binary-example1

[3] J. Mutai. "VLSM Subnetting Explained: How to Subnet by Host Requirements," ComputingForGeeks, cập nhật 17/06/2026. [Online]. Available: https://computingforgeeks.com/subnetting-vlsm-explained/