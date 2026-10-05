---
type: Comparison
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: scales
publishable: true
criteria:
  - "Mặt nạ mạng (network mask)"
  - "Độ vừa khít với nhu cầu"
  - "Áp lực lên bảng định tuyến (routing table)"
  - "Công sức thiết kế"
  - "Phụ thuộc giao thức định tuyến"
Related to:
  - "[[classful-addressing]]"
  - "[[classless-addressing]]"
Belongs to: []
Has:
  - "[[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]]"
  - "[[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]]"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Địa chỉ có lớp (classful) và địa chỉ không phân lớp (classless)

## Bối cảnh

Hai mô hình cấp phát địa chỉ IP xuất hiện ở hai thời kỳ khác nhau của Internet. Mô hình **[[classful-addressing]]** được đặc tả trong RFC 791 (1981); mô hình **[[classless-addressing]]** ra đời từ thập niên 1990 để sửa những giới hạn của nó. So sánh này giúp phân biệt hai mô hình theo các tiêu chí thiết kế và vận hành mạng.

## Điểm khác biệt cốt lõi

| Tiêu chí | Classful | Classless |
|---|---|---|
| **Mặt nạ mạng (network mask)** | Cố định theo lớp, **suy ra từ địa chỉ** [2] | Tùy ý, **phải khai báo tường minh** [2] |
| **Độ vừa khít với nhu cầu** | Thấp — chỉ ba cỡ `/8, /16, /24`, dễ lãng phí khối lớn [1][2] | Cao — chọn cỡ khối theo nhu cầu [1] |
| **Bảng định tuyến (routing table)** | Phình do nhiều khối rời rạc [1] | **Gom được tuyến** nếu cấp phát liền kề (xem [[cidr]]) [1] |
| **Công sức thiết kế** | Thấp — không cần tính prefix | Cao hơn — phải tính prefix, căn lề khối, ghi chép cấp phát |
| **Phụ thuộc giao thức định tuyến** | Giao thức classful **không mang mặt nạ mạng (network mask)** kèm tuyến | Giao thức phải **truyền kèm mặt nạ mạng (network mask)** |

## Đánh đổi (trade-off) và cơ chế

- **Classful tiện ở chỗ mặt nạ mạng (network mask) suy ra được từ địa chỉ** — không cần khai báo, không cần tính. Cái giá là ba cỡ khối quá thô, dẫn đến lãng phí (xem [[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]]).
- **Classless đổi lấy sự linh hoạt bằng độ phức tạp vận hành:** khi địa chỉ không còn ngụ ý mặt nạ mạng (network mask), người thiết kế phải ghi nhớ và khai báo mặt nạ mạng (network mask) ở mọi nơi. Chi phí thật là **độ phức tạp vận hành** — dễ cấp chồng lấn, dễ sai prefix, và đòi hỏi kế hoạch cấp phát có hệ thống [1].
- **Ràng buộc giao thức:** dòng cuối bảng nghĩa là **RIPv1** (Routing Information Protocol phiên bản 1) chỉ định tuyến theo classful nên **không dùng được với VLSM**, trong khi các giao thức mới hơn mang mặt nạ mạng (network mask) kèm tuyến [1].

## Khi nào dùng cái nào

- **Classful** chỉ còn giá trị lịch sử/học thuật; không dùng cho thiết kế mới.
- **Classless** là mặc định hiện đại, và kỹ năng của nó chuyển sang được cho [[ipv6]].
- Hệ quả thực hành: khi thiết kế địa chỉ, hãy dùng [[chia-subnet-theo-so-host]] và ghi chép cấp phát để tránh chồng lấn.

Xem thêm: [[classful-addressing]], [[classless-addressing]], [[vlsm]], [[cidr]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.