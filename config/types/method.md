---
type: Type
_icon: wrench
_sidebar_label: Methods
_order: 3
color: teal
---

# Method

**Câu hỏi trả lời:** "Làm X thế nào?"

**Bao gồm:** design patterns, workflows, algorithms, checklists, recipes.

**Phân biệt với `examples/`:** method = quy trình tổng quát (reusable); example = một lần thực hiện cụ thể.

**File name:** tên phương pháp — `strangler-fig-pattern.md`

---

## Classification Test

Chọn `Method` nếu unit trả lời “Làm X thế nào?” và:

- có thể thực hiện lặp lại trong nhiều context;
- có input, precondition, hoặc điều kiện kích hoạt rõ;
- có sequence, checklist, algorithm, pattern, hoặc workflow;
- có output hoặc done criteria;
- không chỉ là một recommendation chung.

## Do Not Use This Type For

Không dùng `Method` nếu unit:

- chỉ nói nên/không nên làm gì mà không có quy trình → `Insight`;
- chỉ định nghĩa pattern là gì → `Concept`;
- kể một lần áp dụng cụ thể → `Example`;
- so sánh nhiều approach → `Comparison`.

## Common Confusions

### Method vs Insight

- `Insight`: “Nên làm X.”
- `Method`: “Làm X bằng các bước A, B, C.”

### Method vs Example

- `Method`: reusable process.
- `Example`: một lần áp dụng process trong context cụ thể.

### Method vs Concept

- `Concept`: pattern là gì.
- `Method`: cách áp dụng pattern.

---

## Frontmatter Schema

```yaml
---
type: Method
domain: [tên-domain]
source_type: file          # file | generated | chat
date: YYYY-MM-DD
status: Draft              # Draft | Active | In progress | Done | Archived | Blocked
url: ""                    # tuỳ chọn
_icon: wrench              # Lucide icon — tuỳ chọn
applies_when: "Điều kiện kích hoạt phương pháp này"
steps: []                  # tóm tắt các bước chính (optional — có thể mô tả trong body)
expected_output: ""
Related to: []             # C — Tolaria relationship; concepts/insights liên quan
Belongs to: []             # C — Tolaria relationship; parent/container nếu có
Has: []                    # C — Tolaria relationship; child/component nếu có
sources:                   # C — audit trail tới _sources/
  - id: "source-id"
    ref: "[N]"
---
```

## Body Structure

```markdown
[Method này dùng để đạt mục tiêu gì và trong context nào] [1].

## Điều kiện tiên quyết

- Input cần có: ...
- Điều kiện kích hoạt: ...

## Các bước

1. [Bước 1] [1]
2. [Bước 2]
3. [Bước 3]

## Kết quả / Tiêu chí hoàn tất

[Thành phẩm hoặc dấu hiệu hoàn tất]

## Khi nào không nên dùng

[Điều kiện ngược lại, anti-patterns, hoặc failure modes]

Xem thêm: [[related-method]], [[related-concept]]

## Tài liệu tham khảo

[1] Tác giả, *Tiêu đề*, Nhà xuất bản, Năm.

## Luyện tập

- Tình huống 1 để luyện tập hoặc nhận biết khi nào áp dụng method này?
- Tình huống 2?
```

## Minimal Valid Note

Một `Method` note hợp lệ tối thiểu phải có:

- điều kiện áp dụng;
- steps hoặc workflow có thể thực hiện;
- expected output hoặc done criteria;
- citation cho method hoặc steps chính;
- không chỉ là một principle/recommendation.

## Language & Terminology

Xem `config/types/_conventions.md` — **single source of truth** cho quy tắc ngôn ngữ heading và xử lý thuật ngữ. Không định nghĩa lại ở đây.

Tóm tắt áp dụng cho Method:

- H1 = tên method, tiếng Việt (hoặc `Việt (English)` nếu có bản dịch).
- Heading body = `## Điều kiện tiên quyết` · `## Các bước` · `## Kết quả / Tiêu chí hoàn tất` · `## Khi nào không nên dùng` · `## Tài liệu tham khảo` · `## Luyện tập`.
- Body = thuật ngữ có bản dịch dùng `Việt (English)` ở **mọi lần xuất hiện**; acronym/chuẩn giữ nguyên.
