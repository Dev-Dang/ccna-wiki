---
type: Type
_icon: brain
_sidebar_label: Syntheses
_order: 6
color: red
---

# Synthesis

**Câu hỏi trả lời:** "A + B + C gộp lại cho thấy gì mới?"

**Không sở hữu proposition** — chỉ narrative + links. Mọi proposition nảy sinh khi viết → extract ra `insights/` note riêng.

**`publishable`:** `false` nếu bất kỳ atomic note nào được tổng hợp từ source không có real IEEE citation.

**File name:** mô tả ngắn synthesis — `module-boundaries.md`

---

## Classification Test

Chọn `Synthesis` nếu unit hoặc proposal cần kết hợp nhiều atomic notes để tạo framing/góc nhìn tích hợp và:

- có nhiều components;
- không chỉ là so sánh A/B theo criteria;
- không thể biểu diễn đầy đủ bằng một insight atomic;
- mọi proposition mới đã được extract thành `insights/`.

## Do Not Use This Type For

Không dùng `Synthesis` nếu unit:

- chỉ là một claim atomic → `Insight`;
- so sánh subjects theo criteria rõ → `Comparison`;
- định nghĩa một concept → `Concept`;
- mô tả procedure reusable → `Method`;
- kể một case cụ thể → `Example`.

## Extract Insight Flow

Trước khi draft synthesis:

1. List candidate new propositions.
2. Với mỗi proposition, quyết định:
   - đã được cover bởi insight hiện có;
   - cần create/enrich insight;
   - quá yếu/speculative, omit.
3. Chỉ sau đó draft synthesis bằng narrative + links đến components và extracted insights.

Không draft proposition mới inline rồi mới hy vọng extract sau.

## Common Confusions

### Synthesis vs Insight

- `Insight`: một proposition atomic.
- `Synthesis`: narrative tích hợp nhiều atomic notes, không sở hữu proposition mới.

### Synthesis vs Comparison

- `Comparison`: A vs B theo criteria.
- `Synthesis`: A + B + C tạo framing hoặc hệ hiểu tích hợp.

---

## Frontmatter Schema

```yaml
---
type: Synthesis
domain: [tên-domain]
source_type: file          # file | generated | chat
date: YYYY-MM-DD
status: Draft              # Draft | Active | In progress | Done | Archived | Blocked
url: ""                    # tuỳ chọn
_icon: layers              # Lucide icon — tuỳ chọn
publishable: true          # false nếu bất kỳ atomic note nào dùng fallback citation
Related to: []             # C — Tolaria relationship; notes liên quan
Belongs to: []             # C — Tolaria relationship; parent/container nếu có
Has: []                    # C — Tolaria relationship; components và insights được tổng hợp/extract
sources:                   # C — audit trail tới _sources/
  - id: "source-id"
    ref: "[N]"
  - id: "source-id-2"
    ref: "[N]"
---
```

## Body Structure

```markdown
[Lead paragraph — framing của synthesis, trỏ đến components và extracted insights thay vì tự sở hữu claim mới] [1].

## Khung nhìn

[Giải thích vì sao các components nên được đọc cùng nhau]

## Góc nhìn tích hợp

[Tổng hợp narrative. Mọi claim mạnh phải link đến `extracted_insights`.]

## Hàm ý

- [[insight-1]] — implication chính.
- [[insight-2]] — implication phụ.

## Tài liệu tham khảo

[1] Tác giả, *Tiêu đề*, Nhà xuất bản, Năm.
[2] Tác giả 2, *Tiêu đề 2*, Nhà xuất bản, Năm.
```

## Minimal Valid Note

Một `Synthesis` note hợp lệ tối thiểu phải có:

- nhiều components;
- framing tích hợp rõ;
- mọi proposition mới nằm trong `extracted_insights`;
- body không sở hữu claim mạnh chưa extract;
- citation bubble-up, de-dup, và remap đúng.

## Language & Terminology

Xem `config/types/_conventions.md` — **single source of truth** cho quy tắc ngôn ngữ heading và xử lý thuật ngữ. Không định nghĩa lại ở đây.

Tóm tắt áp dụng cho Synthesis:

- H1 = mô tả ngắn synthesis, tiếng Việt.
- Heading body = `## Khung nhìn` · `## Góc nhìn tích hợp` · `## Hàm ý` · `## Tài liệu tham khảo`.
- Body = thuật ngữ có bản dịch dùng `Việt (English)` ở **mọi lần xuất hiện**; acronym/chuẩn giữ nguyên.

## Index Entry Format (Compound Notes section)

```markdown
- [[ten-synthesis]] — synthesis: [mô tả ngắn — A + B → kết luận gì]
```
