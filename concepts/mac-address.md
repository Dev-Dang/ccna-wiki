---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: true
_icon: box
aliases: ["MAC", "MAC address", "địa chỉ MAC", "địa chỉ phần cứng", "địa chỉ vật lý"]
Related to:
  - "[[frame]]"
  - "[[switch]]"
  - "[[ip-address]]"
  - "[[broadcast-domain]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Địa chỉ MAC (MAC address)

**Địa chỉ MAC (MAC address – Media Access Control address)** là **địa chỉ phần cứng gắn với card mạng** của thiết bị, dài **48 bit**, dùng để **xác định thiết bị khi gửi dữ liệu trong cùng một mạng cục bộ (LAN)** [1].

## Phân biệt MAC và IP

Hai loại địa chỉ này phục vụ hai tầng khác nhau và hay bị lẫn với nhau:

- **Địa chỉ IP** là địa chỉ **"logic"** — có thể đổi theo cấu hình và định danh điểm đầu cuối (endpoint) xuyên mạng.
- **Địa chỉ MAC** là địa chỉ **gắn với phần cứng** — định danh card mạng của thiết bị trong phạm vi một mạng cục bộ (LAN) [1].

Nói ngắn: IP trả lời "thiết bị này ở mạng nào", MAC trả lời "card mạng nào trong mạng cục bộ (LAN) này".

## MAC đi cùng khung dữ liệu (frame) như thế nào

Mỗi **khung dữ liệu (frame)** ở tầng Ethernet mang **MAC nguồn và MAC đích**, còn gói tin IP nằm bên trong khung dữ liệu (frame). **Bộ chuyển mạch (switch)** chuyển khung dữ liệu (frame) dựa trên **MAC đích**, không đọc địa chỉ IP [1].

Trong cơ chế **flood-and-learn — tràn gói tin rồi học địa chỉ**, thiết bị dùng **ARP (Address Resolution Protocol – giao thức phân giải địa chỉ)** để hỏi "địa chỉ IP này ứng với MAC nào?", rồi mới tạo khung dữ liệu (frame) với MAC đích tương ứng.

## MAC chỉ có ý nghĩa trong từng chặng

MAC đích **được thay ở mỗi bộ định tuyến (router)**: nó chỉ có ý nghĩa trong từng chặng (hop) Ethernet, còn **địa chỉ IP đích giữ nguyên suốt đường đi** [1]. Đây là điểm hay bị nhầm khi theo dấu một gói tin đi ra ngoài mạng qua cổng mặc định (default gateway).

Xem thêm: [[frame]], [[switch]], [[ip-address]], [[broadcast-domain]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.
