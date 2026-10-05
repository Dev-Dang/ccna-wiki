---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["PAT", "Port Address Translation", "NAT overload", "dịch địa chỉ cổng", "PAT quá tải"]
Related to:
  - "[[network-address-translation]]"
  - "[[default-gateway]]"
  - "[[router]]"
  - "[[private-ip-address]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Dịch địa chỉ cổng (PAT – Port Address Translation)

**Dịch địa chỉ cổng (PAT – Port Address Translation)**, còn gọi là **NAT overload**, là dạng NAT **viết lại cả địa chỉ IP lẫn số cổng (port)**, cho phép **nhiều thiết bị dùng chung một địa chỉ public** [1].

## Vì sao phải viết cả số cổng

Dạng NAT cơ bản chỉ đổi địa chỉ IP: mỗi session (luồng kết nối) cần một cặp (địa chỉ IP, số cổng) riêng. Một địa chỉ public chỉ có khoảng **65 nghìn số cổng** dùng được cho TCP/UDP [1]. Nếu chỉ đổi IP, số kết nối đồng thời bị giới hạn ở vài chục nghìn — không đủ khi cả một văn phòng dùng chung một địa chỉ public.

PAT giải quyết bằng cách **ghép số cổng vào định danh phiên**: router biên mạng giữ bảng (IP private + cổng nội bộ) → (IP public + cổng ngoại vi đã phân bổ). Nhiều thiết bị dùng chung IP public, nhưng **mỗi phiên có một cổng ngoại vi riêng** trên địa chỉ public đó [1].

## PAT khác gì NAT thường

| | NAT thường | PAT (NAT overload) |
|---|---|---|
| Viết lại | Chỉ địa chỉ IP | Địa chỉ IP **và** số cổng |
| Số phiên đồng thời trên 1 IP public | Bị giới hạn | Hàng chục nghìn (theo cổng) |
| Ghi chú | Đủ cho vài thiết bị, ít session | Chuẩn phổ biến cho mạng gia đình/văn phòng |

*Nguồn bảng và giải thích tổng hợp từ phần PAT trong tài liệu [1].*

## PAT và cổng mặc định (default gateway)

Ở mạng gia đình/văn phòng nhỏ, bộ định tuyến (router) Wi-Fi thường vừa là **[[default-gateway]]** cho host, vừa làm **PAT** ở mặt ngoài [1]. Hai vai trò này **có thể do cùng một thiết bị đảm nhận, nhưng không phải luôn luôn** — trong mạng lớn, PAT có thể do firewall/[[router]] khác đảm nhiệm.

Xem thêm: [[network-address-translation]], [[default-gateway]], [[private-ip-address]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.