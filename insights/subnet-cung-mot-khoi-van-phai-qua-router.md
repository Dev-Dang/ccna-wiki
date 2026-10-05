---
type: Insight
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: lightbulb
claim: "Chia một khối địa chỉ thành nhiều subnet không tạo ra kết nối trực tiếp: hai subnet cùng khối vẫn phải đi qua router, vì ranh giới do mask quyết định chứ không do khối gốc."
confidence: high
Related to:
  - "[[ip-network]]"
  - "[[broadcast-domain]]"
  - "[[router]]"
  - "[[host]]"
  - "[[default-gateway]]"
  - "[[network-mask]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# Chia một khối địa chỉ thành nhiều subnet không tạo ra kết nối trực tiếp giữa chúng

## Bối cảnh

Khi cần chia một khối địa chỉ được cấp (ví dụ `192.168.1.0/24`) thành nhiều mạng con (subnet) nhỏ hơn, một câu hỏi rất hay gặp: *"Các mạng con (subnet) này vốn cùng một khối, vậy chúng có nói chuyện trực tiếp với nhau được không?"* Trực giác mách bảo "có, vì chúng ở gần nhau". Trực giác đó **sai**.

## Luận điểm

**Chia một khối thành nhiều mạng con (subnet) không tạo ra kết nối trực tiếp giữa chúng.** Hai mạng con (subnet) chia ra từ **cùng một khối địa chỉ** vẫn **bắt buộc phải giao tiếp qua một hoặc nhiều bộ định tuyến (router)**. Việc "cùng khối gốc" không có ý nghĩa gì với đường đi của gói tin [1].

## Cơ chế

Ranh giới giữa các mạng con (subnet) do **mặt nạ mạng (network mask)** quyết định, không do khối gốc:

- **Khối được cấp (allocation)** là dải địa chỉ tổ chức nhận được — ví dụ `192.168.1.0/24`.
- **Mạng con (subnet)** là dải con với ranh giới do **mặt nạ mạng (network mask)** quyết định — ví dụ `192.168.1.0/26` và `192.168.1.64/26`.

Khi **[[host]]** muốn gửi dữ liệu, nó **tự phán quyết** đích là cùng mạng hay khác mạng bằng **phép AND bit**: áp mặt nạ mạng (network mask) của chính mình lên địa chỉ nguồn và địa chỉ đích, rồi so hai kết quả [1].

- **Bằng nhau →** cùng mạng con (subnet): gửi trực tiếp qua [[switch]], không cần bộ định tuyến (router).
- **Khác nhau →** khác mạng con (subnet): gửi cho **[[default-gateway]]** để **[[router]]** chuyển tiếp.

Mỗi mạng con (subnet) cũng là một **miền quảng bá (broadcast domain)** riêng, và [[router]] là ranh giới chặn quảng bá (broadcast) [1]. Chia một khối thành bốn mạng con (subnet) nghĩa là tạo ra **bốn miền quảng bá (broadcast domain)** — ARP trong mạng con này không vói tới mạng con kia.

## Phạm vi / ngoại lệ

- **Quyết định dựa trên mặt nạ mạng (network mask), không dựa trên vị trí vật lý:** hai host cắm chung một bộ chuyển mạch (switch) nhưng khác mạng con (subnet) vẫn phải gửi nhau qua cổng mặc định (default gateway) [1].
- **"Router" ở đây là chức năng, không nhất thiết là một hộp riêng:** trong thiết kế hiện đại, vai trò định tuyến có thể do **Layer 3 switch** đảm nhận [1].
- **Để cách ly quảng bá (broadcast) giữa các mạng con (subnet), chức năng định tuyến phải nằm trên VLAN riêng** — nếu không, miền quảng bá (broadcast domain) lại bị nối liền [1].

Xem ví dụ số cụ thể: [[chia-mot-khoi-lam-bon-subnet-26]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.