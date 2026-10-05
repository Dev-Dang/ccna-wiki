---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["default gateway", "cổng mặc định", "gateway"]
Related to:
  - "[[router]]"
  - "[[host]]"
  - "[[ip-network]]"
  - "[[network-mask]]"
  - "[[network-address-translation]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Cổng mặc định (default gateway)

**Cổng mặc định (default gateway)** là **địa chỉ IP của một interface của bộ định tuyến (router) nằm trong cùng mạng con (subnet) với host**. Host gửi **mọi gói tin có đích ngoài mạng của mình** đến địa chỉ này [1].

## Vì sao host cần cổng mặc định (default gateway)

Bộ định tuyến (router) đọc IP đích và tra **bảng định tuyến (routing table)**; nhưng host không giữ bảng định tuyến đầy đủ. Thay vào đó, host chỉ cần một "cửa ra" mặc định: sau khi áp **mặt nạ mạng (network mask)** bằng **phép AND bit** mà thấy đích **khác mạng**, host gửi gói cho cổng mặc định (default gateway) [1].

Một **[[host]]** cần tối thiểu **ba thông số** để dùng IP bình thường: **địa chỉ IP**, **mặt nạ mạng (network mask)**, và **cổng mặc định (default gateway)**. Thiếu cổng mặc định (default gateway), host vẫn nói chuyện được với host cùng mạng, nhưng **không ra được ngoài mạng** [1].

## Điểm hay bị nhầm: IP đích so với MAC đích

Khi đi qua cổng mặc định (default gateway):

- **IP đích của gói tin vẫn là đích cuối cùng** (ví dụ `8.8.8.8`).
- **MAC đích của khung dữ liệu (frame) là MAC của cổng mặc định (default gateway)**, không phải MAC của máy đích.

Lý do: MAC chỉ có ý nghĩa trong từng chặng (hop) Ethernet và được thay ở mỗi bộ định tuyến (router), còn IP đích giữ nguyên suốt đường đi [1].

## Cổng mặc định (default gateway) và thiết bị NAT không phải lúc nào cũng trùng nhau

Ở nhà hoặc văn phòng nhỏ, bộ định tuyến (router) Wi-Fi thường đảm nhận **cả hai** vai trò: cổng mặc định (default gateway) và thiết bị làm **[[network-address-translation]]** [1]. Trong mạng lớn thì không nhất thiết: cổng mặc định (default gateway) của một VLAN có thể là một Layer 3 switch bên trong, còn NAT được làm ở firewall hay bộ định tuyến (router) ngoài rìa [1].

Xem thêm: [[router]], [[host]], [[ip-network]], [[network-mask]], [[network-address-translation]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.
