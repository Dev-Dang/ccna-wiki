---
type: Synthesis
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: true
_icon: layers
publishable: true
Related to:
  - "[[ip-address]]"
  - "[[broadcast-domain]]"
  - "[[classless-addressing]]"
Belongs to: []
Has:
  - "[[mot-mien-quang-ba-khong-the-mo-rong-thanh-mot-mang-ip-khong-lo]]"
  - "[[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]]"
  - "[[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]]"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Ràng buộc kép dẫn đến chia mạng con

## Khung nhìn

Toàn bộ chuyện chia mạng con (subnetting) xuất phát từ một **ràng buộc kép**: **không gian địa chỉ hữu hạn** và **cơ chế quảng bá không mở rộng được** [1][2]. Hai ràng buộc này giải thích vì sao mô hình địa chỉ phải đổi từ [[classful-addressing]] sang [[classless-addressing]], và vì sao ta phải tính toán prefix thay vì cấp phát tùy tiện.

Đọc riêng từng mảnh — địa chỉ IP, miền quảng bá, mặt nạ mạng, classful/classless — sẽ thấy chúng rời rạc. Đọc cùng nhau, chúng tạo thành một mạch lập luận nhân quả liền mạch.

## Góc nhìn tích hợp

Mạch lập luận:

1. **[[ip-address]]** cho thấy không gian IPv4 chỉ có `2³²` địa chỉ và đã cạn → mọi địa chỉ phải được dùng khéo.
2. **[[broadcast-domain]]** cho thấy flood-and-learn — tràn gói tin rồi học địa chỉ — qua ARP không mở rộng được → không thể gom mọi thiết bị vào một mạng khổng lồ ([[mot-mien-quang-ba-khong-the-mo-rong-thanh-mot-mang-ip-khong-lo]]).
3. **[[classful-addressing]]** là nỗ lực đầu tiên nhóm địa chỉ thành mạng, nhưng ba cỡ khối quá thô → lãng phí ([[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]]).
4. **[[classless-addressing]]** (với **[[vlsm]]** và **[[cidr]]**) sửa cả hai vấn đề: cắt khối vừa khít nhu cầu, và gom tuyến để hãm phình bảng định tuyến (routing table) ([[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]]).
5. Các cơ chế kéo dài tuổi thọ (**[[private-ip-address]]**, **[[network-address-translation]]**, CGNAT) giảm nhẹ áp lực địa chỉ, nhưng phá vỡ kết nối đầu-cuối ([[nat-pha-vo-ket-noi-dau-cuoi-cua-ung-dung]]); **[[ipv6]]** mới giải quyết tận gốc.

## Hàm ý

- [[mot-mien-quang-ba-khong-the-mo-rong-thanh-mot-mang-ip-khong-lo]] — vì sao phải chia mạng thay vì dùng một mạng khổng lồ.
- [[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]] — vì sao ba cỡ khối cứng không đủ.
- [[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]] — vì sao CIDR không chỉ để tiết kiệm địa chỉ.
- Kỹ năng hệ quả: [[chia-subnet-theo-so-host]] (quy trình) áp dụng lên các case cụ thể như [[chia-200-1-1-0-24-thanh-32-16-16-host]].

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.