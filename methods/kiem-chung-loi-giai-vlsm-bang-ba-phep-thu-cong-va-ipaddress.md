---
type: Method
domain: networking
source_type: file
date: 2026-10-06
status: Draft
_organized: false
_icon: wrench
applies_when: "Đã có một lời giải chia mạng con (subnet) — dù tự tính hay chép từ nguồn khác — và cần xác nhận nó không sai trước khi đem cấu hình."
steps:
  - "Kiểm tra mỗi Subnet ID chia hết cho block size của chính nó"
  - "Kiểm tra broadcast của mạng trước + 1 = Subnet ID của mạng sau"
  - "Cộng tổng kích thước các khối, xác nhận ≤ kích thước khối gốc"
  - "Chạy lại toàn bộ bằng thư viện ipaddress của Python"
expected_output: "Xác nhận lời giải nằm trong khối gốc, không chồng lấn, và tổng địa chỉ đã cấp không vượt khối — kèm bảng trống còn lại."
Related to:
  - "[[sau-cach-giai-bai-toan-vlsm]]"
  - "[[chia-subnet-theo-so-host]]"
  - "[[vlsm]]"
Belongs to: []
Has: []
sources:
  - id: "vlsm-zero"
    ref: "[1]"
    paragraph: "§Kiểm chứng đáp án và lỗi hay gặp"
  - id: "vlsm-zero"
    ref: "[2]"
    paragraph: "§Kiểm chứng đáp án và lỗi hay gặp"
---

# Kiểm chứng lời giải VLSM bằng ba phép thủ công và thư viện ipaddress

## Điều kiện tiên quyết

- Có **danh sách Subnet ID** kèm prefix của từng mạng con (subnet).
- Biết **khối gốc** và kích thước của nó.
- Biết **block size** (`2ⁿ`) của từng mạng — công cụ để phép kiểm 1 chạy được.

Phương pháp này áp dụng cho **mọi** lời giải, kể cả lời giải tự tính, kể cả đáp án chép từ một trang hướng dẫn nổi tiếng.

## Các bước

### 1. Ba phép kiểm thủ công

**Phép 1 — Subnet ID chia hết cho block size.** Với mỗi mạng, lấy Subnet ID chia cho block size của chính nó; phải là số nguyên.

Ví dụ `0, 64, 128, 160, 176` chia hết lần lượt cho `64, 64, 32, 16, 4`. ✓

**Phép 2 — Không chồng lấn.** Địa chỉ quảng bá (broadcast) của mạng trước **cộng 1** phải bằng Subnet ID của mạng sau.

`63 + 1 = 64`, `127 + 1 = 128`, `159 + 1 = 160`, `175 + 1 = 176`. ✓ [2]

Phép này tương đương cách phát hiện chồng lấn bằng mắt: **nếu broadcast của mạng con trước ≥ Subnet ID của mạng con sau thì chúng chồng lấn** [2].

**Phép 3 — Tổng không vượt khối gốc.** Cộng kích thước tất cả các khối đã cấp; tổng phải ≤ kích thước khối gốc.

`64 + 64 + 32 + 16 + 4 = 180 ≤ 256`, còn dư 76. ✓

### 2. Kiểm tra bằng máy

Ba phép trên làm được bằng tay, nhưng khi có nhiều mạng thì nên để máy kiểm. Thư viện chuẩn `ipaddress` của Python làm được cả ba trong vài dòng:

```python
import ipaddress as ip

base = ip.ip_network("192.168.40.0/24")
plan = ["192.168.40.0/26", "192.168.40.64/26", "192.168.40.128/27",
        "192.168.40.160/28", "192.168.40.176/30"]
nets = [ip.ip_network(p) for p in plan]   # sai Subnet ID sẽ báo ValueError

assert all(n.subnet_of(base) for n in nets)
assert not any(a.overlaps(b) for i, a in enumerate(nets) for b in nets[i+1:])
print(sum(n.num_addresses for n in nets), "đã cấp")
```

Đoạn code này kiểm cả ba phép cùng lúc:

- `ip.ip_network(p)` sẽ ném `ValueError` nếu Subnet ID lệch ranh giới (phép 1).
- `n.subnet_of(base)` xác nhận mọi khối nằm trong khối gốc.
- `a.overlaps(b)` kiểm cặp không chồng lấn (phép 2).
- `sum(n.num_addresses)` cho tổng đã cấp, đem so với `base.num_addresses` (phép 3).

Kết quả mong đợi với đáp án chuẩn: cả 5 mạng nằm trong `/24`, không chồng lấn, dùng 180 và còn trống 76.

## Kết quả / Tiêu chí hoàn tất

- Cả ba phép thủ công đều ✓ cho **mọi** mạng trong danh sách.
- Script `ipaddress` chạy không lỗi và in ra tổng đã cấp đúng như tính tay.
- Biết được **phần trống còn lại** và hình dạng của nó — xem [[phuong-an-vlsm-hop-le-khac-toi-uu-uu-tien-khoi-trong-lien-ke-lon-nhat]].

## Khi nào không nên dùng

Phương pháp này chỉ **xác nhận**, không sửa lỗi giúp bạn. Nếu phép kiểm trượt, phải quay lại đặt Subnet ID theo [[sau-cach-giai-bai-toan-vlsm]].

Ngoài ra, **năm lỗi hay gặp** sau đây không phải lúc nào phép kiểm cũng bắt được ngay — nên biết để tránh từ đầu [1][2]:

- **Quên trừ 2** và chọn khối đúng bằng số host. Ví dụ 15 host cần khối **32**, vì khối 16 chỉ có 14 dùng được [1].
- **Cấp sai thứ tự** (khối nhỏ trước), gây phân mảnh hoặc không đủ khối căn đúng cho mạng lớn [2].
- **Subnet ID lệch ranh giới**, ví dụ đặt khối 32 tại `.144` [2].
- **Sai ±1 ở broadcast.** Công thức đúng: `broadcast = Subnet ID + block − 1`.
- **Chép đáp án mà không kiểm tra.** Ngay trên một trang hướng dẫn nổi tiếng [3], dòng cuối của bảng tổng kết ghi host dùng được của `192.168.1.108/30` là `.107 – .108`, trong khi đúng phải là `.109 – .110` (vì Subnet ID `.108` và broadcast `.111`). Lỗi nhỏ nhưng cho thấy lý do phải tự kiểm tra [1].

Xem thêm: [[sau-cach-giai-bai-toan-vlsm]], [[chia-subnet-theo-so-host]], [[phuong-an-vlsm-hop-le-khac-toi-uu-uu-tien-khoi-trong-lien-ke-lon-nhat]]

## Tài liệu tham khảo

[1] NetworkAcademy.io. "VLSM without Binary Example 1." [Online]. Available: https://www.networkacademy.io/ccna/ip-subnetting/vlsm-without-binary-example1

[2] J. Mutai. "VLSM Subnetting Explained: How to Subnet by Host Requirements," ComputingForGeeks, cập nhật 17/06/2026. [Online]. Available: https://computingforgeeks.com/subnetting-vlsm-explained/

[3] L. Goswami. "VLSM Subnetting Examples and Calculation Explained," ComputerNetworkingNotes, cập nhật 10/05/2026. [Online]. Available: https://www.computernetworkingnotes.com/ccna-study-guide/vlsm-subnetting-examples-and-calculation-explained.html

## Luyện tập

- Chạy lại đoạn script trên với plan cố ý sai (`192.168.40.144/27` cho BRANCH LAN1) — lỗi ném ra ở dòng nào, và nó tương ứng phép kiểm nào?
- Với đáp án chuẩn, tính phần trống còn lại bằng `ipaddress` (gợi ý: trừ các khối đã cấp khỏi khối gốc) rồi so với `.180/30`, `.184/29`, `.192/26`.