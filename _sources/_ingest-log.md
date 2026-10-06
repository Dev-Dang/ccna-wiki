# Ingest Log — `_sources/`

| File | Source ID | Copied | Validated | Notes Created | Status |
|------|-----------|--------|-----------|---------------|--------|
| why-do-we-need-ip-subnetting-vi.md | why-do-we-need-ip-subnetting-vi | 2026-10-05 | ✓ | 21 | done |
| ip-addressing-learn.md | ip-addressing-learn | 2026-10-05 | ✓ | 21 | done |
| ip-addressing-learn.md (v2 re-ingest) | ip-addressing-learn | 2026-10-05 | ✓ | 14 | done |
| vlsm-zero.md | vlsm-zero | 2026-10-06 | ✓ | 5 | done |
| static-routing-zero.md | static-routing-zero | 2026-10-06 | ✓ | 0 | done |

> 21 notes tạo từ cặp nguồn trùng chủ đề (bản dịch NetworkAcademy.io + note distill).
> Cả hai nguồn đóng góp vào cùng bộ notes — xem frontmatter `sources` của từng note.
> Citation nguồn A (bản dịch) dùng NetworkAcademy.io theo xác nhận của user.
>
> **Re-ingest v2 (`ip-addressing-learn.md` → version: 2, 2026-10-05):**
> - Ghi đè file `_sources/ip-addressing-learn.md` theo yêu cầu user (ngoại lệ §3 CLAUDE.md).
> - Ref [1] giữ nguyên tiêu đề, mô tả nguồn đổi thành *"tài liệu người dùng cung cấp; không ghi rõ tác giả"* theo v2.
> - Tạo mới: 6 concept (`mac-address`, `frame`, `switch`, `router`, `default-gateway`, `host`), 1 concept (`special-ip-blocks`), 1 concept (`loopback-address`), 1 concept (`port-address-translation`), 1 insight (`subnet-cung-mot-khoi-van-phai-qua-router`), 1 method (`tra-cuu-prefix-va-buoc-nhay`), 2 example (`chia-mot-khoi-lam-bon-subnet-26`, `and-bit-voi-mask-khong-tron-octet`) = **14 notes mới**.
> - ENRICH: `broadcast-domain`, `ip-network`, `network-address-translation`, `classful-50-cua-hang-lang-phi-25600-dia-chi`.
> - Tổng notes còn liên kết nguồn này: 35 (21 cũ + 14 mới; 4 note được enrich thêm nội dung v2).
> - Không có unit bị SKIP (U15 — ghi chú chất lượng v1 — trở nên thừa sau khi file cũ bị ghi đè).

> **Ingest `vlsm-zero.md` (2026-10-06):** Verdict PROCEED, source quality high, technical confidence high.
> - Tạo mới: 2 insight (`chia-mang-khong-tao-dia-chi-moi-chi-phan-vung-lai-khoi`, `phuong-an-vlsm-hop-le-khac-toi-uu-uu-tien-khoi-trong-lien-ke-lon-nhat`), 2 method (`sau-cach-giai-bai-toan-vlsm`, `kiem-chung-loi-giai-vlsm-bang-ba-phep-thu-cong-va-ipaddress`), 1 example (`vlsm-br1-dat-tai-96-27-chong-lan-voi-hq-lan2`) = **5 notes mới**.
> - ENRICH: `concepts/vlsm.md` (bốn luật bất biến), `concepts/network-mask.md` (định nghĩa prefix `/n`), `methods/tra-cuu-prefix-va-buoc-nhay.md` (công thức ngược `prefix = 32 − ⌈log₂(host + 2)⌉`).
> - SKIP: `/31 vs /30` — đã có `insights/prefix-31-tiet-kiem-dia-chi-cho-link-diem-diem`.

> **Soạn `static-routing-zero.md` (2026-10-06):** Verdict PROCEED (bài tổng hợp foundation-zero, chưa ingest thành atomic notes).
> - Nguồn: `Chapter04_Routing.ppt` (slide 1–81, đã trích text sang `D:\tmp-ppt-extract\slide-text.txt`) + 2 nguồn web.
> - Nguồn web: NetworkLessons.com "Route Summarization" (công thức gộp tuyến, ví dụ `192.168.0.0/22`); ComputerNetworkingNotes "ip route Command Explained with Examples" (cú pháp, AD, next-hop vs exit interface); Wikipedia "Hop (networking)" (hop, next-hop, bảng định tuyến, phân giải tầng liên kết); Wikipedia "Stub network" (stub network/transit network, analog hòn đảo, stub AS).
> - Các lần WebSearch đều không trả kết quả dùng được (echo rỗng hoặc lỗi tên tool); các nguồn web lấy được qua WebFetch trực tiếp. Nhiều URL Cisco/computernetworkingnotes trả 404 — 2 URL trong References là URL đã fetch thành công.
> - Khoảng trống nội dung: slide 53 nêu mục tiêu "Describe summary and default route" nhưng **không slide nào triển khai summary route** → phần tuyến tổng hợp dựa hoàn toàn vào nguồn web [2].
> - Ghi chú: 0 atomic notes tạo mới — đây là bài tổng hợp dạng `-zero.md`, không phải ingest phân loại.
> - ENRICH đề xuất (chưa thực hiện): `concepts/router.md` (ba loại tuyến, bảng định tuyến), `concepts/cidr.md` (gom tuyến thực hành), có thể tạo mới `concepts/static-route.md`, `concepts/default-route.md`, `concepts/stub-network.md`, `concepts/next-hop.md`, `methods/cau-hinh-static-route-packet-tracer.md`, `insights/summary-route-rong-hon-tap-mang-that.md`, `insights/stub-router-nen-dung-default-static-route.md` khi thực hiện ingest.
> - **Gia cố lần 1 (2026-10-06, `--zero --web`):**
>   - Thêm mục `## Các thuật ngữ thường gặp trong định tuyến tĩnh` — bảng 22 thuật ngữ + 3 tiểu mục: `### Hop, next-hop và mô hình chỉ đường` (hop, next-hop, tra cứu đệ quy, ràng buộc chuỗi liền mạch), `### Stub network và stub router` (stub network/transit network, stub AS, phân biệt với `stub area` của OSPF), `### Tại sao Cisco khuyên dùng default static route cho stub router` (4 lý do + cảnh báo phạm vi + heuristic đếm số lối thoát).
>   - Thay `## Luyện tập` (7 đề trơ) bằng `## Lời giải bài tập` — mỗi bài có **Đề bài** + **Lời giải** đầy đủ (Bài 1 serial `up`; Bài 2 hai dạng tuyến tĩnh; Bài 3 gộp tuyến nhẩm vs nhị phân, cảnh báo shortcut `256−n`; Bài 4 sửa tuyến sai cổng; Bài 5 đọc `debug ip routing`; Bài 6 stub router một dòng; Bài 7 longest prefix match trên router trung tâm).
>   - Frontmatter cập nhật: `questions: 16`, `bloom-level: 4`; thêm nguồn [4] Wikipedia "Hop (networking)", [5] Wikipedia "Stub network".
>   - **Tồn đọng:** mục "Tại sao Cisco khuyên dùng..." suy ra từ [1] + [5] và gắn nhãn `Suy luận:` — chưa có văn bản Cisco chính thức làm nguồn trực tiếp. Nếu user cung cấp tài liệu Cisco gốc, thay bằng citation trực tiếp.