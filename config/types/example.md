---
type: Type
_icon: clipboard-text
_sidebar_label: Examples
_order: 4
color: orange
_sort: "modified:desc"
---

# Example

**Câu hỏi trả lời:** "X trông thế nào trong thực tế?"

**Quy tắc cứng:** Mỗi example PHẢI link ít nhất một trong ba:
- `illustrates` → một concept
- `supports` → một insight
- `demonstrates` → một method

Example không có link = không hợp lệ.

**`lesson`:** bắt buộc khi và chỉ khi `supports` rỗng.

**File name:** mô tả ngắn trường hợp — `refactor-payment-strangler.md`

---

## Classification Test

Chọn `Example` nếu unit mô tả một case cụ thể và:

- có actor, system, context, hoặc situation cụ thể;
- có diễn biến, quyết định, outcome, hoặc observation quan sát được;
- minh hoạ concept, hỗ trợ insight, hoặc demonstrate method;
- không tự nó là generalized claim.

## Do Not Use This Type For

Không dùng `Example` nếu unit:

- chỉ định nghĩa thuật ngữ → `Concept`;
- phát biểu claim tổng quát → `Insight`;
- mô tả procedure reusable → `Method`;
- so sánh A/B theo criteria → `Comparison`.

## Lesson Rule

`lesson` bắt buộc khi `supports` rỗng.

Nếu `lesson` có thể đứng độc lập như một claim tổng quát, không giữ nó như proposition riêng trong example. Thay vào đó:

1. tạo/enrich một `Insight`;
2. thêm insight đó vào `supports`;
3. để `lesson` chỉ tóm tắt local takeaway của case.

## Common Confusions

### Example vs Insight

- `Example`: evidence/case cụ thể.
- `Insight`: claim tổng quát rút ra từ một hoặc nhiều examples.

### Example vs Method

- `Example`: một lần thực hiện.
- `Method`: quy trình có thể tái sử dụng.

---

## Frontmatter Schema

```yaml
---
type: Example
domain: [tên-domain]
source_type: file          # file | generated | chat
date: YYYY-MM-DD
status: Draft              # Draft | Active | In progress | Done | Archived | Blocked
url: ""                    # tuỳ chọn — link case study/bài viết gốc
_icon: clipboard-list      # Lucide icon — tuỳ chọn
Related to: []             # C — Tolaria relationship; concepts/insights/methods mà example minh họa hoặc hỗ trợ
Belongs to: []             # C — Tolaria relationship; parent/container nếu có
Has: []                    # C — Tolaria relationship; child/component nếu có
lesson: ""                 # bắt buộc nếu supports rỗng
sources:                   # C — audit trail tới _sources/
  - id: "source-id"
    ref: "[N]"
    paragraph: ""
---
```

## Body Structure

Thứ tự trình bày: **example đầy đủ trước, giải thích sau**.

Người đọc phải thấy toàn bộ case trước khi đọc phân tích — không xen kẽ giải thích vào giữa example.

1. **Bối cảnh** — đủ để hiểu example xảy ra trong điều kiện nào (1-2 câu).
2. **Tình huống** — code snippet nguyên vẹn, hoặc mô tả case hoàn chỉnh.
3. **Giải thích** — phân tích từng phần, outcome, ý nghĩa.

```markdown
# [mô tả ngắn trường hợp — đồng bộ với filename]

[Bối cảnh: 1-2 câu context đủ để hiểu example]

[Tình huống — code hoặc case nguyên vẹn]

[Giải thích — phân tích, outcome, ý nghĩa]

Xem thêm: [[related-insight]], [[related-method]]

## Tài liệu tham khảo

[1] Tác giả, *Tiêu đề*, Nhà xuất bản, Năm.
```

Không bắt buộc heading nào. Note phải đứng độc lập: người đọc không cần mở source file mới hiểu được case.

## Code Extraction Rule

Nếu source có code snippet là phần cốt lõi của case:
- Trích nguyên code vào note body.
- Không thay bằng mô tả prose như "Nguồn đưa đoạn code gồm s1, s2...".
- Có thể thêm chú thích inline trong code nếu giúp đọc hiểu.

## Minimal Valid Note

Một `Example` note hợp lệ tối thiểu phải có:

- case cụ thể đủ để đọc hiểu độc lập (không cần mở source);
- code snippet nếu source có và code là phần cốt lõi;
- ít nhất một trong `illustrates`, `supports`, `demonstrates`;
- outcome hoặc observation;
- `lesson` nếu `supports` rỗng;
- citation cho case.

## Language & Terminology

Xem `config/types/_conventions.md` — **single source of truth** cho quy tắc ngôn ngữ heading và xử lý thuật ngữ. Không định nghĩa lại ở đây.

Tóm tắt áp dụng cho Example:

- H1 = mô tả ngắn trường hợp, tiếng Việt.
- Heading body = `## Bối cảnh` · `## Tình huống` · `## Giải thích` · `## Tài liệu tham khảo`.
- Body = thuật ngữ có bản dịch dùng `Việt (English)` ở **mọi lần xuất hiện**; acronym/chuẩn giữ nguyên.
