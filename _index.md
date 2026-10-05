---
updated: 2026-10-05
---

# Wiki Index

Mục lục tra cứu toàn bộ nội dung trong kho tri thức.

---

## Compound Notes

*(Comparisons và Syntheses — flat list, không phân domain)*

- [[classful-vs-classless]] — comparison: địa chỉ có lớp (classful) và không phân lớp (classless) theo mặt nạ mạng (network mask), độ vừa khít, bảng định tuyến (routing table), công sức thiết kế, giao thức;
  *keywords: classful, classless, comparison, mặt nạ mạng (network mask), bảng định tuyến (routing table)*
- [[rang-buoc-kep-dan-den-chia-mang-con]] — synthesis: IPv4 hữu hạn + quảng bá (broadcast) không mở rộng được → classless, CIDR, NAT, IPv6;
  *keywords: chia mạng con (subnetting), ràng buộc kép, IPv4, classless, CIDR*

---

## Concepts

*(X là gì?)*

- [[ip-address]] — địa chỉ IP là định danh điểm đầu cuối (endpoint); IPv4 32 bit, không gian hữu hạn;
  *keywords: ip address, địa chỉ IP, IPv4, điểm đầu cuối (endpoint), octet*
- [[host]] — host là máy tính/điện thoại/máy chủ có địa chỉ IP; cần IP, mask, gateway;
  *keywords: host, thiết bị đầu cuối, ba thông số, endpoint*
- [[ip-network]] — nhóm địa chỉ cùng miền quảng bá (broadcast domain), không cần bộ định tuyến (router) để giao tiếp;
  *keywords: ip network, mạng IP (IP network), mạng con (subnet), bộ định tuyến (router), cổng mặc định (default gateway)*
- [[broadcast-domain]] — phạm vi mọi thiết bị nhận được bản sao gói quảng bá (broadcast); chặn tại bộ định tuyến (router);
  *keywords: broadcast domain, miền quảng bá (broadcast domain), ARP, flood-and-learn, VLAN*
- [[mac-address]] — địa chỉ phần cứng 48 bit gắn với card mạng; định danh thiết bị trong mạng cục bộ (LAN);
  *keywords: MAC address, địa chỉ MAC, địa chỉ phần cứng, 48 bit, LAN*
- [[frame]] — khung dữ liệu (frame) tầng Ethernet, mang MAC nguồn/đích, gói IP nằm bên trong;
  *keywords: frame, khung dữ liệu, Ethernet, MAC, đóng gói*
- [[switch]] — bộ chuyển mạch (switch) chuyển khung dữ liệu (frame) theo MAC đích, không đọc IP;
  *keywords: switch, bộ chuyển mạch, LAN, flood-and-learn, Layer 3 switch*
- [[router]] — bộ định tuyến (router) nối các mạng IP, đọc IP đích và tra bảng định tuyến (routing table);
  *keywords: router, bộ định tuyến, định tuyến IP, bảng định tuyến (routing table), ranh giới quảng bá (broadcast)*
- [[default-gateway]] — cổng mặc định (default gateway) là interface bộ định tuyến (router) cùng mạng con (subnet) với host;
  *keywords: default gateway, cổng mặc định, gateway, mạng con (subnet), NAT*
- [[network-mask]] — mặt nạ mạng (network mask) cho biết phần nào là mạng, phần nào là host; AND bit;
  *keywords: network mask, subnet mask, mặt nạ mạng (network mask), AND bit, mặt nạ mạng con (subnet mask)*
- [[classful-addressing]] — mô hình lớp A/B/C/D/E, mặt nạ mạng (network mask) cố định, ba cỡ khối;
  *keywords: classful, lớp A/B/C/D/E, địa chỉ có lớp, RFC 791*
- [[classless-addressing]] — địa chỉ không ngụ ý mặt nạ mạng (network mask), khai báo tường minh; cỡ khối tùy ý;
  *keywords: classless, địa chỉ không phân lớp, VLSM, CIDR*
- [[vlsm]] — mặt nạ mạng (network mask) có độ dài thay đổi, chia một khối thành mạng con (subnet) cỡ khác nhau;
  *keywords: VLSM, variable-length subnet masking, chia mạng con (subnetting)*
- [[cidr]] — định tuyến liên miền không phân lớp, prefix tùy ý, gom tuyến (route aggregation);
  *keywords: CIDR, classless inter-domain routing, prefix, route aggregation, RFC 1519/4632*
- [[private-ip-address]] — ba khối private RFC 1918, shared 100.64/10 không định tuyến Internet;
  *keywords: private address, RFC 1918, 100.64/10, địa chỉ private*
- [[special-ip-blocks]] — bảng khối địa chỉ đặc biệt: dự trữ, loopback, private, shared, link-local, multicast, lớp E;
  *keywords: special-use addresses, khối địa chỉ đặc biệt, link-local, 169.254, CGNAT, multicast*
- [[loopback-address]] — địa chỉ loopback để thiết bị nói chuyện với chính nó; 127.0.0.0/8, localhost;
  *keywords: loopback, 127.0.0.1, localhost, loopback interface*
- [[network-address-translation]] — NAT đổi private→public, CGNAT NAT hai lớp;
  *keywords: NAT, network address translation, CGNAT, NAT hai lớp*
- [[port-address-translation]] — PAT (NAT overload) viết lại cả địa chỉ lẫn số cổng, nhiều thiết bị chung một IP public;
  *keywords: PAT, NAT overload, dịch địa chỉ cổng, port, giới hạn cổng*
- [[ipv6]] — địa chỉ 128 bit, RFC 2460/8200, giải quyết cạn kiệt IPv4;
  *keywords: IPv6, 128-bit, RFC 2460, RFC 8200*

## Insights

*(Điều gì đúng / nên làm?)*

- [[mot-mien-quang-ba-khong-the-mo-rong-thanh-mot-mang-ip-khong-lo]] — quảng bá (broadcast) không mở rộng được ở quy mô lớn, không thể một mạng IP khổng lồ;
  *keywords: quảng bá (broadcast), miền quảng bá (broadcast domain), flood-and-learn, quy mô lớn*
- [[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]] — classful lãng phí địa chỉ vì ba cỡ khối quá thô;
  *keywords: classful, lãng phí, ba cỡ khối, 25600 địa chỉ*
- [[cidr-gom-tuyen-lam-cham-toc-do-phinh-bang-dinh-tuyen]] — CIDR gom tuyến (route aggregation) để hãm phình bảng định tuyến (routing table);
  *keywords: CIDR, gom tuyến (route aggregation), bảng định tuyến (routing table), gom tuyến*
- [[prefix-31-tiet-kiem-dia-chi-cho-link-diem-diem]] — /31 cho phép dùng cả hai địa chỉ trên link điểm-điểm (point-to-point) (RFC 3021);
  *keywords: /31, prefix 31, RFC 3021, điểm-điểm (point-to-point), /32*
- [[nat-pha-vo-ket-noi-dau-cuoi-cua-ung-dung]] — NAT phá vỡ kết nối đầu-cuối (end-to-end), ứng dụng nhúng IP trong phần dữ liệu (payload) khó hoạt động;
  *keywords: NAT, kết nối đầu-cuối (end-to-end), phần dữ liệu (payload), VoIP, IPsec, CGNAT*
- [[subnet-cung-mot-khoi-van-phai-qua-router]] — chia một khối thành nhiều mạng con (subnet) không tạo kết nối trực tiếp; ranh giới do mask quyết định;
  *keywords: subnet, cùng khối, bộ định tuyến (router), mask, AND bit, miền quảng bá (broadcast domain)*

## Methods

*(Làm X thế nào?)*

- [[chia-subnet-theo-so-host]] — quy trình cắt một khối thành mạng con (subnet) vừa khít số host, cấp khối lớn trước, căn lề 2ᵏ;
  *keywords: chia mạng con (subnetting), VLSM, mạng con (subnet), căn lề, phương pháp*
- [[tra-cuu-prefix-va-buoc-nhay]] — tra nhanh số host của prefix và tính ranh giới subnet bằng bước nhảy (256 − octet mask);
  *keywords: prefix, bước nhảy, magic number, /26, /30, AND bit, tra cứu*

## Examples

*(X trông thế nào trong thực tế?)*

- [[classful-50-cua-hang-lang-phi-25600-dia-chi]] — công ty 50 cửa hàng, 500 host, bị classful bắt dùng 25.600 địa chỉ;
  *keywords: classful, 50 cửa hàng, lãng phí, LAN C*
- [[chia-200-1-1-0-24-thanh-32-16-16-host]] — chia 200.1.1.0/24 thành /26, /27, /27 cho 32/16/16 host;
  *keywords: VLSM, chia mạng con (subnetting), /24, /26, /27, 200.1.1.0*
- [[chia-mot-khoi-lam-bon-subnet-26]] — chia 192.168.1.0/24 thành bốn /26; hai subnet cùng khối vẫn phải qua router;
  *keywords: /26, AND bit, bộ định tuyến (router), miền quảng bá (broadcast domain), cùng khối*
- [[and-bit-voi-mask-khong-tron-octet]] — AND bit 10.1.1.2/255.255.224.0 → 10.1.0.0/19; bước nhảy 32;
  *keywords: AND bit, mask không tròn octet, /19, bước nhảy, 8.190 host*
