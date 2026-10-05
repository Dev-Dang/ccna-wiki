---
type: Type
_icon: toolbox
_sidebar_label: Concepts
_order: 1
color: green
_sort: "title:asc"
---

# Concept

**Câu hỏi trả lời:** "X là gì?"

**Chứa:** định nghĩa, phân loại con, bản chất ontology, ranh giới, phân biệt với khái niệm gần.

**Không chứa:** nhận định đúng/sai, recommendation, bước thực hiện, ví dụ phức tạp, hoặc case cụ thể.

**File name:** tên khái niệm, kebab-case — `dependency-injection.md`

---

## Classification Test

Chọn `Concept` nếu unit chủ yếu trả lời “X là gì?” và:

- định nghĩa một thuật ngữ, entity, property, process, abstraction, pattern, hoặc category;
- có thể đặt tên bằng một noun phrase;
- ý chính không phải benefit, causal effect, trade-off, hoặc recommendation;
- không mô tả chuỗi bước thực hiện;
- không phụ thuộc vào một case cụ thể.

## Do Not Use This Type For

Không dùng `Concept` nếu unit:

- nói “X dẫn đến Y” → `Insight`;
- nói “nên làm X thay vì Y” → `Insight`;
- mô tả cách làm X → `Method`;
- kể một tình huống cụ thể → `Example`;
- so sánh A/B theo tiêu chí → `Comparison`;
- kết hợp nhiều notes thành framing mới → `Synthesis`.

## Common Confusions

### Concept vs Insight

- `Concept`: định nghĩa X là gì.
- `Insight`: phát biểu điều gì đúng hoặc nên làm với X.

Ví dụ:

- “Cohesion là mức độ các responsibility trong module liên quan với nhau.” → `Concept`
- “High cohesion làm giảm chi phí bảo trì.” → `Insight`

### Concept vs Method

- `Concept`: dependency injection là gì.
- `Method`: cách áp dụng constructor injection.

### Concept vs Example

- `Concept`: event sourcing là gì.
- `Example`: một hệ thống banking dùng event sourcing để audit giao dịch.

---

## Frontmatter Schema

```yaml
---
type: Concept
domain: [tên-domain]
source_type: file          # file | generated | chat
date: YYYY-MM-DD
status: Draft              # Draft | Active | In progress | Done | Archived | Blocked
url: ""                    # tuỳ chọn — link nguồn online
_icon: box                 # Lucide icon — tuỳ chọn
aliases: []                # C — Claude keyword matching trong index lookup
Related to: []             # C — Tolaria relationship; concepts/insights liên quan trực tiếp
Belongs to: []             # C — Tolaria relationship; parent/container nếu có
Has: []                    # C — Tolaria relationship; child/component nếu có
sources:                   # C — audit trail tới _sources/
  - id: "source-id"        # → _sources/source-id.md
    ref: "[N]"             # kết nối với [N] trong References section
    paragraph: ""          # tuỳ chọn — tên section trong source file
  # Thêm entries khi note được enrich từ nguồn thứ hai, ba...
---
```

## Body Structure

Thứ tự trình bày theo cognitive scaffolding — tóm tắt cốt lõi trước, chi tiết sau:

1. **Định nghĩa + tóm tắt kiến thức quan trọng nhất** — đọc xong đoạn đầu là nắm được concept.
2. **Diễn giải lần lượt** — cơ chế, cách hoạt động, ví dụ minh hoạ nếu cần.
3. **Ranh giới / phân biệt** — chỉ khi concept dễ nhầm hoặc dễ hiểu sai phạm vi.

```markdown
# [tên khái niệm — tiếng Việt, đồng bộ với filename]

[Định nghĩa ngắn gọn + điểm quan trọng nhất cần nhớ về concept này]

[Diễn giải cơ chế, cách hoạt động — theo mạch tự nhiên, không bắt buộc heading]

[Ranh giới hoặc phân biệt — chỉ khi thực sự cần]

Xem thêm: [[related-concept-1]], [[related-concept-2]]

## Tài liệu tham khảo

[1] Tác giả, *Tiêu đề*, Nhà xuất bản, Năm.
```

Không bắt buộc heading nào. Chỉ thêm heading khi nội dung đủ dài để cần phân đoạn.

## Language & Terminology

Xem `config/types/_conventions.md` — **single source of truth** cho quy tắc ngôn ngữ heading và xử lý thuật ngữ. Không định nghĩa lại ở đây.

Tóm tắt áp dụng cho Concept:

- H1 = `Việt (English)`: `# Miền quảng bá (broadcast domain)`, đồng bộ với `aliases`.
- Heading body = nhãn cấu trúc tiếng Việt, không bọc ngoặc.
- Body = thuật ngữ có bản dịch dùng `Việt (English)` ở **mọi lần xuất hiện**; acronym/chuẩn giữ nguyên.

## Undefined Term Rule

Khi body dùng term kỹ thuật quan trọng chưa có concept note trong wiki, phải chọn một trong hai:

(a) Giải thích inline ngắn gọn bằng `(mô tả)` ngay sau term:
- `heap (vùng nhớ động nơi JVM cấp phát object)`
- `stack frame (khung ngăn xếp chứa biến cục bộ của một lần gọi method)`

(b) Đánh dấu `[[term]]` và ghi vào proposal/note cuối: "Cần tạo concept note cho [[term]]".

Không để term kỹ thuật quan trọng trôi nổi không giải thích trong body.

## Minimal Valid Note

Một `Concept` note hợp lệ tối thiểu phải có:

- định nghĩa rõ ràng;
- citation cho định nghĩa;
- ranh giới hoặc phân biệt nếu concept dễ nhầm;
- không chứa claim/recommendation làm ý chính.

## Index Entry Format

```markdown
- [[ten-concept]] — mô tả ngắn;
  *keywords: keyword1, keyword2, thuật ngữ khác*
```
