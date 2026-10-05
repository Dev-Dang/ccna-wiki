---
type: Convention
_icon: spell-check
_sidebar_label: Conventions
_order: 99
---

# Conventions — Ngôn ngữ heading & thuật ngữ

**File này là single source of truth cho quy tắc ngôn ngữ trong mọi note của wiki.**
Mọi schema trong `config/types/*.md` tham chiếu về đây; không định nghĩa lại để tránh lệch.

---

## 1. Ngôn ngữ của heading — canonical tiếng Việt

Áp dụng cho **mọi heading body** (H2 trở xuống) trong mọi type.

- Heading là **nhãn cấu trúc**, không phải thuật ngữ chuyên môn → **không bọc ngoặc**, không kèm `(English)`.
- Heading vẫn có thể **chứa** thuật ngữ chuyên môn khi thuật ngữ chính là chủ đề (ví dụ `## Ranh giới: classful vs classless`) — trường hợp đó giữ nguyên dạng gốc.

### Bảng ánh xạ chuẩn (heading tiếng Anh → tiếng Việt)

| Heading cũ (EN / xen kẽ) | Heading chuẩn (VI) | Dùng ở type |
|---|---|---|
| `## References` | `## Tài liệu tham khảo` | tất cả |
| `## Tài liệu tham khảo` | `## Tài liệu tham khảo` | tất cả |
| `## Setup` | `## Bối cảnh` | example |
| `## Case` | `## Tình huống` | example |
| `## Giải thích` | `## Giải thích` | example |
| `## Bối cảnh` | `## Bối cảnh` | insight, comparison, example |
| `## Claim` | `## Luận điểm` | insight |
| `## Cơ chế` / `## Cơ chế / lý do` | `## Cơ chế` | insight |
| `## Bằng chứng định lượng` | `## Bằng chứng định lượng` | insight |
| `## Ví dụ điển hình` | `## Ví dụ điển hình` | insight |
| `## Phạm vi` / `## Phạm vi / ngoại lệ` | `## Phạm vi / ngoại lệ` | insight |
| `## Preconditions` | `## Điều kiện tiên quyết` | method |
| `## Steps` | `## Các bước` | method |
| `## Output / Done criteria` | `## Kết quả / Tiêu chí hoàn tất` | method |
| `## Khi nào không nên dùng` | `## Khi nào không nên dùng` | method |
| `## Practice` | `## Luyện tập` | method |
| `## Framing` | `## Khung nhìn` | synthesis |
| `## Integrated view` | `## Góc nhìn tích hợp` | synthesis |
| `## Implications` | `## Hàm ý` | synthesis |
| `## Điểm khác biệt cốt lõi` | `## Điểm khác biệt cốt lõi` | comparison |
| `## Trade-offs và cơ chế` | `## Đánh đổi (trade-off) và cơ chế` | comparison |
| `## Khi nào dùng cái nào` | `## Khi nào dùng cái nào` | comparison |

### Heading H1

- **Concept / Method** → H1 là **tên khái niệm thuật ngữ**: dạng `Việt (English)` nếu có bản dịch tự nhiên, hoặc giữ English/acronym nếu là tên riêng. Đồng bộ với `aliases`.
  - `# Miền quảng bá (broadcast domain)` · `# Mặt nạ mạng (network mask)` · `# CIDR (Classless Inter-Domain Routing)`
  - **Acronym-first exception:** khi thuật ngữ được nhận diện chủ yếu qua acronym/tên riêng và bản dịch tiếng Việt ít dùng trong thực hành, giữ H1 dạng `# Acronym (Tên đầy đủ tiếng Anh)` — ví dụ `# NAT (Network Address Translation)` · `# VLSM (Variable-Length Subnet Masking)` · `# CIDR (Classless Inter-Domain Routing)` · `# IPv6`. Lý do: đây là **tên riêng của chuẩn/giao thức**, không phải thuật ngữ mô tả cần dịch ở tiêu đề; bản dịch tiếng Việt (nếu có) đặt ở `aliases` và dùng thống nhất trong body.
    - Phân biệt: `classful/classless addressing`, `broadcast domain`, `network mask` là **thuật ngữ mô tả** → H1 `Việt (English)`. Còn `NAT`, `CIDR`, `VLSM`, `IPv6` là **tên riêng/acronym** → H1 acronym-first.
- **Insight** → H1 là **câu claim đầy đủ** (theo Heading Test của `insight.md`), tiếng Việt.
- **Example / Comparison / Synthesis** → H1 tiếng Việt, mô tả nội dung.

---

## 2. Quy tắc thuật ngữ trong body

Theo `doc-translator/references/terminology-rules.md` — **nguyên tắc "không bao giờ drop bản dịch"**.

### Loại 1 — Có bản dịch Việt tự nhiên

Dạng `Việt (English)` ở **mọi lần xuất hiện**, không bỏ bản dịch ở lần sau.

| English | Dạng chuẩn |
|---|---|
| broadcast domain | miền quảng bá (broadcast domain) |
| IP network | mạng IP (IP network) |
| router | bộ định tuyến (router) |
| default gateway | cổng mặc định (default gateway) |
| network mask | mặt nạ mạng (network mask) |
| subnetting | chia mạng con (subnetting) |
| routing table | bảng định tuyến (routing table) |
| route aggregation | gom tuyến (route aggregation) |
| payload | phần dữ liệu (payload) |
| header | phần tiêu đề (header) |
| point-to-point | điểm-điểm (point-to-point) |
| endpoint | điểm đầu cuối (endpoint) |
| subnet mask | mặt nạ mạng con (subnet mask) |

Trong domain `networking`, các từ `host`, `subnet`, `switch`, `router` được cộng đồng chuyên ngành dùng nguyên vẹn — dùng đúng dạng Loại 1 khi cần giải thích lần đầu trong một note mới.

### Loại 2 — Jargon / acronym giữ nguyên

**Không** bọc ngoặc, **không** dịch:

`NAT` · `CGNAT` · `CIDR` · `VLSM` · `ARP` · `MAC` · `VLAN` · `DHCP` · `DNS` · `ISP` · `IANA` · `RIR` · APNIC/LACNIC/ARIN/AfriNIC/RIPE NCC · `RFC 1918` / `RFC 3021` / `RFC 6598` · `IPv4` / `IPv6` · `Ethernet` · `host` · `subnet` · `loopback` · `multicast` · `prefix` · `octet` · `broadcast` · `Classful` / `Classless` (dạng tính từ, giữ nguyên khi đứng riêng).

Nếu acronym cần giải thích lần đầu, mở rộng **một lần** dạng `NAT (Network Address Translation – dịch địa chỉ mạng)`, sau đó dùng `NAT`.

### Loại 3 — Term rủi ro dịch sai lĩnh vực

Dùng `English — mô tả chức năng` thay vì dịch thẳng:

- `flood-and-learn` → `flood-and-learn — tràn gói tin rồi học địa chỉ`

### Cấm hybrid

Không tạo cụm nửa Việt nửa Anh trong một từ ghép.

| Sai | Đúng |
|---|---|
| `hệ thống an toàn-critical` | `hệ thống an toàn tới hạn (safety-critical)` |
| `mô hình classful-lãng phí` | `mô hình classful lãng phí` |

---

## 3. Ranh giới với skill `ingest`

`ingest` yêu cầu giữ English-first jargon khi dịch gây hiểu lầm. File này **củng cố** rule đó bằng cách liệt kê tường minh hai nhóm:

- **Nhóm giữ English-first:** acronym, tên chuẩn/giao thức, tên riêng (Loại 2).
- **Nhóm Việt-first kèm English trong ngoặc:** mọi thuật ngữ có bản dịch tự nhiên (Loại 1).

Acronym có bản dịch Việt nhưng cộng đồng chuyên ngành dùng English (`router`, `host`, `subnet`) xếp vào Loại 1 với dạng `Việt (English)`, để vừa đọc được tiếng Việt vừa đối chiếu được tài liệu gốc.
