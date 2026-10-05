---
type: Method
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: wrench
applies_when: "Cần cắt một khối địa chỉ (ví dụ một mạng /24 được cấp) thành các mạng con (subnet) vừa khít với số host mà từng phần trong liên mạng cần."
steps:
  - "Xác định số host cần cho mỗi mạng con (subnet)"
  - "Cộng 2 (địa chỉ mạng + quảng bá) để ra số địa chỉ tối thiểu"
  - "Chọn prefix /n nhỏ nhất sao cho 2^(32−n) ≥ số địa chỉ tối thiểu"
  - "Sắp mạng con (subnet) theo kích thước giảm dần, cấp khối lớn nhất trước"
  - "Căn lề: khối 2^k phải bắt đầu tại bội số của 2^k"
  - "Gán dải địa chỉ, để dành phần dư cho tăng trưởng"
expected_output: "Một bảng phân bổ mạng con (subnet) không chồng lấn, mỗi mạng con (subnet) đủ host khả dụng, tận dụng tối đa khối được cấp."
Related to:
  - "[[vlsm]]"
  - "[[classless-addressing]]"
  - "[[network-mask]]"
Belongs to: []
Has:
  - "[[chia-200-1-1-0-24-thanh-32-16-16-host]]"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Chia mạng con theo số host (VLSM)

Dùng khi ta có một khối địa chỉ đã được cấp và cần cắt nó thành nhiều mạng con (subnet) có kích thước **khác nhau**, mỗi mạng con (subnet) vừa khít với số host của một phần trong liên mạng. Đây là ứng dụng trực tiếp của [[vlsm]].

## Điều kiện tiên quyết

- Đã biết **khối địa chỉ gốc** được cấp (ví dụ `200.1.1.0/24`).
- Đã biết **số host cần** cho từng mạng con (subnet).
- Khối gốc đủ lớn để chứa tổng nhu cầu.

## Các bước

1. **Xác định số host cần** cho mỗi mạng con (subnet). Cẩn thận phân biệt "số host" với "số địa chỉ".
2. **Cộng 2** vào số host để ra số địa chỉ tối thiểu (địa chỉ mạng + địa chỉ quảng bá bị giữ lại). Ví dụ 32 host → cần **34** địa chỉ.
3. **Chọn prefix `/n` nhỏ nhất** sao cho `2^(32−n) ≥ số địa chỉ tối thiểu`. Số host khả dụng của mạng con (subnet) là `2^(32−n) − 2`. Với 34 địa chỉ → cần 64 → `/26`.
4. **Sắp xếp mạng con (subnet) theo kích thước giảm dần** và **cấp khối lớn nhất trước**. Cách này giảm nguy cơ phân mảnh khối.
5. **Căn lề khối:** mỗi khối kích thước `2ᵏ` phải **bắt đầu tại bội số của `2ᵏ`**. Ví dụ khối 64 địa chỉ phải bắt đầu tại địa chỉ chia hết cho 64.
6. **Gán dải địa chỉ** cho từng mạng con (subnet), ghi rõ địa chỉ mạng, dải host khả dụng, địa chỉ quảng bá (broadcast address). **Để dành phần dư** của khối gốc cho tăng trưởng hoặc link WAN.

## Kết quả / Tiêu chí hoàn tất

- Một bảng phân bổ mà **các mạng con (subnet) không chồng lấn** nhau.
- Mỗi mạng con (subnet) có **đủ host khả dụng** so với nhu cầu.
- Không có khối nào lãng phí quá mức (khối được cấp là kích thước nhỏ nhất đủ dùng).
- Phần dư của khối gốc được ghi chú rõ.

## Khi nào không nên dùng

- **Link điểm-điểm (point-to-point):** đừng máy móc trừ 2 địa chỉ. Prefix `/31` (RFC 3021) cho phép dùng cả hai địa chỉ — xem [[prefix-31-tiet-kiem-dia-chi-cho-link-diem-diem]].
- **Giao thức định tuyến classful cũ:** [[vlsm]] không dùng được với giao thức định tuyến theo classful, vì chúng không mang mặt nạ mạng (network mask) kèm tuyến.
- Khi số mạng con (subnet) lớn và cấp phát thủ công dễ chồng lấn — cần quy trình và ghi chép có hệ thống.

Xem thêm: [[vlsm]], [[classless-addressing]], [[chia-200-1-1-0-24-thanh-32-16-16-host]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

## Luyện tập

- Chia `192.168.10.0/24` cho ba phòng ban cần lần lượt 100, 50, 20 host. Bạn chọn prefix nào cho mỗi phòng?
- Với `172.16.0.0/22` (1024 địa chỉ), cần cấp 4 mạng con (subnet) `/26` và 2 mạng con (subnet) `/24`. Kiểm tra các khối có căn lề và không chồng lấn không.