---
type: Type
_icon: scales
_sidebar_label: Comparisons
_order: 5
color: purple
_sort: "modified:desc"
---

# Comparison

**Câu hỏi trả lời:** "A và B khác nhau thế nào theo tiêu chí Z?"

**Không sở hữu proposition** — kết luận đủ mạnh phải được extract ra `insights/`.

**`publishable`:** `false` nếu bất kỳ atomic note nào được tổng hợp từ source không có real IEEE citation.

**File name:** mô tả so sánh — `event-sourcing-vs-crud.md`

---

## Classification Test

Chọn `Comparison` nếu unit chủ yếu trả lời “A và B khác nhau thế nào theo tiêu chí Z?” và:

- có ít nhất hai subjects;
- có criteria so sánh rõ;
- mục tiêu là phân tích khác biệt hoặc trade-off;
- conclusion mạnh được link đến `insights/`, không sở hữu trực tiếp.

## Do Not Use This Type For

Không dùng `Comparison` nếu unit:

- chỉ là một claim atomic về A tốt hơn B trong context C → `Insight`;
- chỉ định nghĩa A hoặc B → `Concept`;
- mô tả cách chọn/thực hiện theo steps → `Method`;
- tích hợp nhiều notes thành framing không dựa trên criteria so sánh → `Synthesis`.

## Proposition Rule

Comparison không sở hữu proposition mới.

Nếu trong quá trình viết xuất hiện conclusion kiểu:

- “A phù hợp hơn B khi...”;
- “A dẫn đến...”;
- “Nên chọn A nếu...”.

thì conclusion đó phải được tạo/enrich thành `Insight` và link trong frontmatter `conclusion`.

## Common Confusions

### Comparison vs Insight

- `Comparison`: container phân tích A/B theo criteria.
- `Insight`: conclusion atomic rút ra từ comparison.

### Comparison vs Synthesis

- `Comparison`: đặt subjects cạnh nhau theo criteria.
- `Synthesis`: kết hợp nhiều notes để tạo framing hoặc integrated view.

---

## Frontmatter Schema

```yaml
---
type: Comparison
domain: [tên-domain]
source_type: file          # file | generated | chat
date: YYYY-MM-DD
status: Draft              # Draft | Active | In progress | Done | Archived | Blocked
url: ""                    # tuỳ chọn
_icon: scale               # Lucide icon — tuỳ chọn
publishable: true          # false nếu bất kỳ atomic note nào dùng fallback citation
criteria: []               # C — các tiêu chí so sánh
Related to: []             # C — Tolaria relationship; subjects hoặc notes liên quan
Belongs to: []             # C — Tolaria relationship; parent/container nếu có
Has: []                    # C — Tolaria relationship; conclusions và components được tổng hợp
sources:                   # C — audit trail tới _sources/
  - id: "source-id"
    ref: "[N]"
---
```

## Cấu trúc body

Không có bố cục cố định. LLM tự quyết định cách trình bày phù hợp nhất với nội dung so sánh cụ thể.

Nội dung cần trích xuất và tổng hợp — theo thứ tự ưu tiên:

1. **Bối cảnh so sánh** — tại sao cần so sánh A và B, phạm vi áp dụng của comparison này.
2. **Điểm khác biệt cốt lõi** — những gì thực sự phân biệt A và B theo các criteria đã chọn. Có thể dùng bảng, prose, hoặc kết hợp tùy mức độ phức tạp.
3. **Đánh đổi (trade-off) và cơ chế** — giải thích tại sao sự khác biệt đó tồn tại, hệ quả thực tế là gì.
4. **Khi nào dùng cái nào** — điều kiện hoặc tín hiệu để nghiêng về A hoặc B. Nếu có insight đã extract, link đến đó thay vì viết lại.
5. **Tài liệu tham khảo** — bubble-up từ atomic notes, de-dup và remap citation number.

Gợi ý chọn format:

- Dùng **bảng** khi criteria độc lập nhau và mỗi ô có thể điền ngắn gọn.
- Dùng **prose** khi criteria có quan hệ nhân quả hoặc cần giải thích cơ chế liên tục.
- Dùng **kết hợp** khi có một số criteria dạng factual (phù hợp bảng) và một số cần phân tích sâu hơn (phù hợp prose).

Không viết conclusion mới chưa có trong frontmatter `conclusion`. Nếu phát hiện conclusion mới trong quá trình viết, extract thành `Insight` trước rồi link vào `conclusion`.

```markdown
# [Tên so sánh — tiếng Việt, mô tả rõ A vs B]

[Bối cảnh: tại sao so sánh này quan trọng, phạm vi áp dụng]

[Nội dung so sánh — bảng, prose, hoặc kết hợp tùy nội dung]

[Trade-offs và cơ chế — chỉ khi cần giải thích thêm]

[Khi nào dùng cái nào — link đến insights trong `conclusion` thay vì viết lại]

## Tài liệu tham khảo

[1] Tác giả, *Tiêu đề*, Nhà xuất bản, Năm.
[2] Tác giả 2, *Tiêu đề 2*, Nhà xuất bản, Năm.
```

## Note hợp lệ tối thiểu

Một `Comparison` note hợp lệ tối thiểu phải có:

- ít nhất hai subjects;
- criteria rõ;
- body không sở hữu conclusion mạnh chưa extract;
- `conclusion` link đến insights nếu có recommendation/claim;
- citation bubble-up từ atomic notes hoặc source thật.

## Language & Terminology

Xem `config/types/_conventions.md` — **single source of truth** cho quy tắc ngôn ngữ heading và xử lý thuật ngữ. Không định nghĩa lại ở đây.

Tóm tắt áp dụng cho Comparison:

- H1 = tên so sánh, tiếng Việt, mô tả rõ A vs B.
- Heading body = `## Bối cảnh` · `## Điểm khác biệt cốt lõi` · `## Đánh đổi (trade-off) và cơ chế` · `## Khi nào dùng cái nào` · `## Tài liệu tham khảo`.
- Body = thuật ngữ có bản dịch dùng `Việt (English)` ở **mọi lần xuất hiện**; acronym/chuẩn giữ nguyên.

## Format entry trong index (mục Compound Notes)

```markdown
- [[ten-comparison]] — comparison: [A] vs [B] theo [tiêu chí chính]
```
