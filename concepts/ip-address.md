---
type: Concept
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: box
aliases: ["IP address", "địa chỉ IP", "IPv4 address"]
Related to:
  - "[[ip-network]]"
  - "[[network-mask]]"
Belongs to: []
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Địa chỉ IP (IP address)

**Địa chỉ IP (IP address — Internet Protocol address)** là **định danh của một điểm đầu cuối (endpoint)** trong các giao tiếp dựa trên giao thức IP. Mỗi thiết bị muốn gửi hoặc nhận dữ liệu qua mạng IP đều cần ít nhất một địa chỉ IP, giống như mỗi chiếc điện thoại cần một số điện thoại để gọi và nhận cuộc gọi: khi thiết bị gửi dữ liệu, nó gửi tới địa chỉ IP của thiết bị đầu bên kia, và thiết bị nhận thấy địa chỉ của thiết bị gửi.

## IPv4 dùng 32 bit

Địa chỉ **IPv4** dài **32 bit** (4 byte), và được viết thành bốn số thập phân cách nhau bằng dấu chấm (ví dụ `10.4.21.43`), mỗi số ứng với một byte — mỗi byte gọi là một **octet**.

Vì dùng đúng 32 bit, không gian địa chỉ IPv4 chứa chính xác:

> 2³² = **4.294.967.296** địa chỉ

Đây là một **tài nguyên hữu hạn (finite resource)**: không thể tạo thêm địa chỉ IPv4 mới. Toàn bộ mọi chuyện về chia mạng con về sau đều bắt đầu từ ràng buộc này — phải dùng từng địa chỉ cho khéo.

*Ví dụ sư phạm thường dùng: không gian IPv4 giống như quỹ đất của một hành tinh — có hạn, và ta phải quy hoạch thay vì cấp phát tùy tiện.*

## Vì sao "hữu hạn" quan trọng: địa chỉ đã cạn

Không gian IPv4 được xem là đã cạn kiệt, nhưng cần nói chính xác hơn cách nói chung chung "cạn kiệt năm 2017":

- **IANA** (Internet Assigned Numbers Authority – cơ quan cấp phát số hiệu Internet) cấp hai khối `/8` chưa dự trữ cuối cùng cho APNIC ngày **31/01/2011**, rồi chia năm khối `/8` còn lại cho năm RIR ngày **03/02/2011** [1].
- Sau đó, **từng RIR** (Regional Internet Registry – cơ quan đăng ký Internet khu vực) cạn kho địa chỉ cấp phát thông thường ở các thời điểm khác nhau: APNIC 15/04/2011, LACNIC 10/06/2014, ARIN 24/09/2015, AfriNIC 21/04/2017, RIPE NCC 25/11/2019 [1].

✦ *Suy luận:* mốc "tháng 4/2017" mà nhiều tài liệu nhắc tới khớp với **AfriNIC**, không phải một sự kiện toàn cầu. Từ sau đó, địa chỉ mới chỉ còn đến từ địa chỉ thu hồi, danh sách chờ và thị trường chuyển nhượng [1].

→ Điều này dẫn trực tiếp đến câu hỏi trung tâm của chia mạng con (subnetting): địa chỉ phải được **nhóm thành mạng** như thế nào cho hiệu quả.

Xem thêm: [[ip-network]], [[network-mask]], [[private-ip-address]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.