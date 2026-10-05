---
type: Insight
domain: networking
source_type: file
date: 2026-10-05
status: Draft
_organized: false
_icon: lightbulb
claim: "Gom tuyến (route aggregation) làm chậm tốc độ phình của bảng định tuyến (routing table) toàn cầu"
confidence: high
Related to:
  - "[[cidr]]"
  - "[[classless-addressing]]"
Has: []
sources:
  - id: "ip-addressing-learn"
    ref: "[1]"
---

# CIDR gom tuyến (route aggregation) để làm chậm tốc độ phình của bảng định tuyến (routing table) toàn cầu

## Bối cảnh

Trước [[cidr]], mỗi khách hàng của một nhà cung cấp dịch vụ có thể chiếm một tuyến riêng trong bảng định tuyến (routing table) toàn cầu. Với hàng trăm nghìn khách hàng, bảng định tuyến (routing table) phình đến mức khó duy trì. CIDR không chỉ là chuyện cắt khối cho vừa nhu cầu — nó còn là một **chiến lược định tuyến**.

## Luận điểm

**Gom tuyến (route aggregation)** cho phép **nhà cung cấp quảng bá một tuyến tổng hợp duy nhất đại diện cho nhiều khách hàng**, thay vì một tuyến cho mỗi khách hàng [1]. Điều này làm **giảm số tuyến** phải lưu và xử lý, qua đó **hãm tốc độ phình của bảng định tuyến (routing table)**.

CIDR ra đời năm **1993** (**RFC 1519**, sau được thay bởi **RFC 4632**) chính với mục tiêu này [1].

## Cơ chế

Ví dụ: nếu một nhà cung cấp quản lý bốn khối `/24` liền kề `200.1.0.0/24`, `200.1.1.0/24`, `200.1.2.0/24`, `200.1.3.0/24`, thay vì quảng bá **bốn tuyến**, chỉ cần quảng bá **một tuyến** `200.1.0.0/22` — miễn là các khối **liền kề** và cùng thuộc một khách hàng/nhà cung cấp. Số tuyến giảm từ 4 xuống 1, và mức giảm tăng theo độ liền kề của các khối được gom.

## Phạm vi / ngoại lệ

- Gom tuyến **chỉ hiệu quả khi các khối được cấp phát liền kề**. Nếu các khối rời rạc, không gom được.
- Khái niệm **prefix** của CIDR **vẫn được dùng trong [[ipv6]]**, nên kỹ năng này không lỗi thời [1].
- Ranh giới cần phân biệt: CIDR là khái niệm ở **tầng định tuyến toàn cầu**; còn việc cắt một khối nội bộ thành các mạng con (subnet) cỡ khác nhau là **[[vlsm]]**.

Xem thêm: [[cidr]], [[classless-addressing]], [[vlsm]]

## Tài liệu tham khảo

[1] "IPv4: địa chỉ, mạng và chia mạng con — từ classful đến classless," *ip-addressing-learn* (tài liệu người dùng cung cấp; không ghi rõ tác giả), 2026-10-05.