---
type: Type
_icon: lightbulb
_sidebar_label: Insights
_order: 2
color: blue
_sort: "modified:desc"
_width: wide
---

# Insight

**Câu hỏi trả lời:** "Điều gì là đúng về X?" / "Nên làm gì với X?"

**Hai loại hợp lệ:**
- **Descriptive:** "X dẫn đến Y" (quan sát thực tế, causal relation, trade-off, constraint)
- **Normative:** "Nên làm X thay vì Y" (best practice, rule of thumb, recommendation)

**Yêu cầu:** Insight phải đứng độc lập — đúng/sai không phụ thuộc vào context note khác.

**File name:** nội dung phát biểu, không phải topic — `cao-cohesion-giam-chi-phi-bao-tri.md`

---

## Classification Test

Chọn `Insight` nếu unit có thể viết thành một câu claim độc lập và:

- có thể được đánh giá đúng/sai, mạnh/yếu, hoặc đáng tin/không đáng tin;
- diễn tả quan hệ nhân quả, trade-off, constraint, principle, best practice, hoặc recommendation;
- có chủ thể và predicate rõ;
- không chỉ định nghĩa một thuật ngữ;
- không phải chuỗi bước thực hiện;
- không chỉ là một case riêng lẻ.

## Claim Test

Một insight hợp lệ phải qua được các câu hỏi:

1. Có thể viết thành một câu đầy đủ không?
2. Có chủ thể rõ không?
3. Có predicate/khẳng định rõ không?
4. Có thể bị phản biện không?
5. Có scope hoặc điều kiện áp dụng không?

Nếu câu trả lời là “không” cho 1–4, không tạo insight. Nếu thiếu scope, thêm scope vào body hoặc hạ confidence.

## Do Not Use This Type For

Không dùng `Insight` nếu unit:

- chỉ định nghĩa X là gì → `Concept`;
- mô tả cách làm có steps/checklist rõ → `Method`;
- kể case cụ thể → `Example`;
- cần bảng criteria để so sánh A/B → `Comparison`;
- là narrative tích hợp nhiều notes → `Synthesis`.

## Common Confusions

### Insight vs Concept

- `Concept`: “X là gì?”
- `Insight`: “Điều gì đúng/nên làm với X?”

### Insight vs Method

- `Insight`: principle hoặc recommendation.
- `Method`: executable procedure.

Ví dụ:

- “Prefer composition over inheritance.” → `Insight`
- “Refactor inheritance hierarchy into composition bằng các bước...” → `Method`

### Insight vs Example

- `Insight`: generalized claim.
- `Example`: concrete evidence/case.

---

## Frontmatter Schema

```yaml
---
type: Insight
domain: [tên-domain]
source_type: file          # file | generated | chat
date: YYYY-MM-DD
status: Draft              # Draft | Active | In progress | Done | Archived | Blocked
url: ""                    # tuỳ chọn
_icon: lightbulb           # Lucide icon — tuỳ chọn
claim: "Phát biểu chính, viết thành câu đầy đủ"
confidence: high           # high | medium | low
Related to: []             # C — Tolaria relationship; concepts/insights liên quan trực tiếp
Belongs to: []             # C — Tolaria relationship; parent/container nếu có
Has: []                    # C — Tolaria relationship; examples hỗ trợ hoặc child/component nếu có
sources:                   # C — audit trail tới _sources/
  - id: "source-id"
    ref: "[N]"
    paragraph: ""
  # Thêm entries khi enrich từ nguồn thứ hai, ba...
---
```

## Body Structure

Thứ tự trình bày: **context trước, claim sau**.

Người đọc cần biết mình đang ở đâu trước khi nhận insight. Không bắt đầu bằng claim trần — đặt claim vào đúng bối cảnh để nó có trọng lượng.

1. **Bối cảnh** — tình huống, vấn đề, hoặc điều kiện mà insight này áp dụng.
2. **Luận điểm** — phát biểu insight rõ ràng.
3. **Cơ chế** — vì sao claim này đúng (chỉ khi không self-evident).
4. **Phạm vi / ngoại lệ** — chỉ khi claim có nguy cơ bị over-generalize.

```markdown
# [câu claim đầy đủ — đọc là thấy giá trị ngay, đồng bộ với filename]

[Context: tình huống hoặc vấn đề mà insight này giải quyết]

[Claim được diễn giải đầy đủ — theo mạch lập luận của source]

[Cơ chế hoặc phạm vi — chỉ khi cần]

Xem thêm: [[related-insight]], [[supporting-example]]

## Tài liệu tham khảo

[1] Tác giả, *Tiêu đề*, Nhà xuất bản, Năm.
```

Không bắt buộc heading nào. Giữ giọng văn và cấu trúc lập luận của source gốc khi có thể.

## Language & Terminology

Xem `config/types/_conventions.md` — **single source of truth** cho quy tắc ngôn ngữ heading và xử lý thuật ngữ. Không định nghĩa lại ở đây.

Tóm tắt áp dụng cho Insight:

- H1 = câu claim đầy đủ, tiếng Việt (Heading Test).
- Heading body = `## Bối cảnh` · `## Luận điểm` · `## Cơ chế` · `## Phạm vi / ngoại lệ`.
- Body = thuật ngữ có bản dịch dùng `Việt (English)` ở **mọi lần xuất hiện**; acronym/chuẩn giữ nguyên.

## Heading Test — Tiêu đề insight

Tiêu đề `#` phải là câu claim đầy đủ, không phải topic noun phrase.

Kiểm tra: nếu tiêu đề có thể là tên một Wikipedia article → đó là topic, không phải insight title. Rewrite thành predicate sentence.

- Sai: `# StringBuilder toString() và so sánh String`
- Đúng: `# StringBuilder.toString() tạo object mới mỗi lần gọi nên == với literal luôn false`

- Sai: `# Cohesion trong module`
- Đúng: `# Cohesion cao trong module giúp giảm chi phí bảo trì dài hạn`

Filename phải đồng bộ với tiêu đề (kebab-case của câu claim).

## Minimal Valid Note

Một `Insight` note hợp lệ tối thiểu phải có:

- claim độc lập trong frontmatter `claim`;
- body không rộng hơn evidence trong source;
- citation cho claim hoặc mechanism;
- confidence phù hợp với source quality;
- scope hoặc caveat nếu claim có nguy cơ overgeneralize.

## Index Entry Format

```markdown
- [[ten-insight]] — mô tả claim ngắn gọn;
  *keywords: keyword1, keyword2*
```
