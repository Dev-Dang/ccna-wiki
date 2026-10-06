---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: true
_icon: box
aliases: ["broadcast domain", "miền quảng bá", "flood-and-learn"]
Related to:
  - "[[ip-network]]"
  - "[[ip-address]]"
  - "[[switch]]"
  - "[[frame]]"
  - "[[vlan]]"
Belongs to:
  - "[[ip-network]]"
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Miền quảng bá (broadcast domain)

**Miền quảng bá (broadcast domain)** là **phạm vi mà mọi thiết bị trong đó đều nhận được một bản sao của gói tin quảng bá (broadcast)**. Khi một thiết bị gửi quảng bá (broadcast), tất cả các thiết bị khác trong cùng miền quảng bá (broadcast domain) đều nghe thấy gói đó, kể cả những thiết bị không liên quan đến nội dung.

## Cơ chế: flood-and-learn — tràn gói tin rồi học địa chỉ, và ARP

Trong một miền quảng bá (broadcast domain), các thiết bị tìm nhau bằng **ARP (Address Resolution Protocol – giao thức phân giải địa chỉ)**. ARP là cơ chế hỏi "địa chỉ IP này có **địa chỉ MAC (MAC address – địa chỉ lớp liên kết dữ liệu)** nào?", để khi biết IP đích, thiết bị còn biết cần gửi khung Ethernet tới địa chỉ phần cứng nào.

Quá trình này gọi là **flood-and-learn — tràn gói tin rồi học địa chỉ**:

1. Host A muốn nói chuyện với host B trong cùng mạng. A chưa biết địa chỉ MAC (MAC address) của B.
2. A **"tràn" (flood)** một ARP request ra toàn bộ miền quảng bá (broadcast domain): "IP của B có MAC nào?"
3. Mọi host trong miền quảng bá (broadcast domain) đều nhận được gói này, kể cả những host không phải B.
4. **B** trả lời lại bằng địa chỉ MAC (MAC address) của mình.
5. A và B bắt đầu **giao tiếp trực tiếp** qua Ethernet.

*Trực giác:* giống như bạn đứng trong một nhóm nhỏ và hỏi to "Tôi đang tìm Bob" — Bob nghe thấy và đáp lại. Nhưng nếu bạn ở trong một sân vận động đầy người và ai cũng hét lên như vậy, môi trường truyền tin sẽ bị quá tải, không ai giao tiếp được. Đó là lý do miền quảng bá (broadcast domain) phải **bị giới hạn**.

## Cùng mạng con (subnet): đường đi thực tế qua bộ chuyển mạch (switch)

Cơ chế trên chạy trên **phần cứng là bộ chuyển mạch (switch)**. Một tình huống đầy đủ, hai host A (192.168.1.10) và B (192.168.1.20) cùng mạng con (subnet) [1]:

1. A muốn gửi cho B nhưng chưa biết địa chỉ MAC (MAC address) của B → A phát **ARP request** dạng quảng bá (broadcast).
2. **Bộ chuyển mạch (switch) tràn bản sao** ARP request ra tất cả các cổng — đây chính là chữ "flood" trong tên cơ chế.
3. **B nhận được ARP request và trả lời trực tiếp cho A** (unicast), kèm địa chỉ MAC (MAC address) của mình.
4. A tạo khung dữ liệu (frame) với **địa chỉ MAC đích = địa chỉ MAC (MAC address) của B** và gửi đi.

Điểm mấu chốt: **không thiết bị nào trong đường đi này đọc địa chỉ IP** — bộ chuyển mạch (switch) chỉ chuyển khung dữ liệu (frame) theo địa chỉ MAC (MAC address). Vì vậy **không có bộ định tuyến (router)** nào tham gia, dù cả hai host đều có địa chỉ IP đầy đủ.

## Ranh giới và giới hạn

- Một **mạng con (subnet) = một miền quảng bá (broadcast domain) = một VLAN** [1][2].
- **Bộ định tuyến (router) là điểm cắt miền quảng bá (broadcast domain):** bộ định tuyến (router) **không chuyển tiếp gói quảng bá (broadcast)**, thay vào đó nó chọn đường theo bảng định tuyến (routing table) [1]. Nhờ vậy bộ định tuyến (router) chấm dứt cơ chế flood-and-learn giữa các mạng.
- **Quy tắc 3 áp dụng cả khi hai mạng con (subnet) được chia ra từ cùng một khối địa chỉ.** Chia một khối thành nhiều mạng con (subnet) không tạo ra kết nối trực tiếp giữa chúng — mỗi mạng con (subnet) vẫn là một miền quảng bá (broadcast domain) riêng, và đi từ mạng con (subnet) này sang mạng con (subnet) kia vẫn phải qua [[router]]. Chi tiết: [[subnet-cung-mot-khoi-van-phai-qua-router]].
- ✦ *Heuristic:* con số "dưới 255 host mỗi miền quảng bá (broadcast domain)" chỉ là **quy ước thực hành**, không phải giới hạn của chuẩn. Giới hạn thực tế phụ thuộc vào lưu lượng quảng bá (broadcast) và thiết kế VLAN [1][2].

Xem thêm: [[ip-network]], [[ip-address]], [[switch]], [[router]], [[frame]], [[vlan]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.