---
type: Insight
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: lightbulb
claim: "Broadcast không mở rộng được ở quy mô lớn, nên không thể gom mọi thiết bị vào một mạng IP khổng lồ"
confidence: high
Related to:
  - "[[broadcast-domain]]"
  - "[[ip-network]]"
Has:
  - "[[classful-50-cua-hang-lang-phi-25600-dia-chi]]"
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
  - id: "why-do-we-need-ip-subnetting-vi"
    ref: "[2]"
---

# Broadcast không mở rộng được ở quy mô lớn, nên không thể gom mọi thiết bị vào một mạng IP khổng lồ

## Bối cảnh

Một câu hỏi tự nhiên: "Sao không gán cho mọi thiết bị trên thế giới một địa chỉ IP và gom hết vào một mạng IP khổng lồ cho xong? Tại sao phải có mạng con (subnet) và mặt nạ mạng con (subnet mask)?" Câu trả lời nằm ở chi phí của quảng bá.

## Luận điểm

Nếu tồn tại một mạng IP duy nhất trải rộng toàn cầu, thì **mỗi gói quảng bá sẽ được tất cả các host trên thế giới nghe thấy**. Ngay cả khi hai thiết bị ở sát cạnh nhau giao tiếp, gói quảng bá của chúng vẫn phải đi sang tận bên kia hành tinh [1][2].

Cụ thể với **[[broadcast-domain]]**:

- Mỗi ARP request đến **mọi host trong miền quảng bá (broadcast domain)**, kể cả các host chẳng liên quan [1].
- ✦ *Suy luận:* lượng quảng bá (broadcast) tăng theo số host, nên môi trường truyền bị quá tải ở quy mô lớn.
- **Bộ định tuyến (router) là điểm cắt:** nó không chuyển tiếp quảng bá (broadcast), thay vào đó chọn đường theo bảng định tuyến (routing table) [1].

## Cơ chế

Cơ chế **flood-and-learn — tràn gói tin rồi học địa chỉ** của ARP chỉ hiệu quả trong một nhóm nhỏ. Khi số host tăng, xác suất "ồn" quảng bá (broadcast) át hết lưu lượng hữu ích cũng tăng, đến mức không host nào giao tiếp được.

*Ẩn dụ của nguồn:* bạn đứng trong nhóm vài người và hỏi "Tôi đang tìm Bob" thì Bob nghe thấy. Nhưng trong một sân vận động đầy người, ai cũng hét "Tôi đang tìm người X" thì môi trường truyền tin quá tải. Giải pháp là **nhờ một nhân viên trật tự** — nói đúng vị trí — và người đó chuyển lời bằng đường ngắn nhất. **Đó chính là việc bộ định tuyến (router) làm** [2].

## Phạm vi / ngoại lệ

Từ đó suy ra giới hạn thực hành: **miền quảng bá (broadcast domain) chỉ là các phân đoạn Ethernet nhỏ** — nguồn [2] nêu "ít hơn 255 host, trong phạm vi vài trăm mét". Con số này là **quy ước thực hành**, không phải giới hạn của chuẩn; giới hạn thật phụ thuộc lưu lượng quảng bá (broadcast) và thiết kế VLAN [1].

Xem thêm: [[broadcast-domain]], [[ip-network]], [[mo-hinh-classful-lang-phi-dia-chi-vi-ba-co-khoi-qua-tho]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.

[2] "Why do we need IP Subnetting?," Introduction to IPv4 Subnetting, NetworkAcademy.io.