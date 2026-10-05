---
_organized: true
---
# CLAUDE.md — Wiki Working Instructions

> Tài liệu này dành riêng cho thư mục `wiki/`.
> Claude đọc file này trước khi làm bất cứ việc gì trong wiki.

---

## 1. Vai trò của wiki

`wiki/` là **kho tri thức đã tiêu hoá** — không phải nơi lưu trữ thô.
Input vào wiki là các file MD đã được xử lý sẵn.

**Nguyên tắc bất biến:**
- `insights/` là source of truth — mọi proposition sống ở đây.
- Compound notes (`comparisons/`, `syntheses/`) chỉ LINK đến insights, không sở hữu proposition mới.
- Một note = một loại = một ý. Không nhập nhằng, không nhồi nhét.

---

## 2. Cấu trúc thư mục

```text
wiki/
├── CLAUDE.md               ← file này
├── _index.md               ← mục lục theo domain
├── _sources/               ← IMMUTABLE: file MD nguồn, không được sửa
│   └── _ingest-log.md
├── concepts/
├── insights/
├── methods/
├── examples/
├── comparisons/
└── syntheses/
```

Skill configs (type schemas) nằm tại `config/types/` trong chính vault này.

---

## 3. `_sources/` — Kho nguồn bất biến

`_sources/` chứa file MD đầu vào trước khi được phân loại vào wiki.

**Quy tắc bất biến:**
- Claude KHÔNG được sửa, xoá, hay ghi đè bất cứ file nào trong `_sources/`.
- Chỉ có một thao tác hợp lệ: **copy file vào**.
- File duy nhất trong `_sources/` được ghi thêm là `_sources/_ingest-log.md`.
- Mọi note trong wiki phải truy ngược về ít nhất một source id trong `_sources/` hoặc provenance tương đương được ghi rõ.

`_sources/_ingest-log.md` dùng format:

```markdown
| File | Source ID | Copied | Validated | Notes Created | Status |
|------|-----------|--------|-----------|---------------|--------|
| ten-file.md | ten-file | 2026-05-01 | ✓ | 3 | done |
```

Status hợp lệ: `done` | `pending` | `partial` | `skipped`.

---

## 4. Các loại note trong wiki

**Single source of truth cho schema:** `config/types/<typename>.md`

| Type | Câu hỏi trả lời | Tính chất | Bloom |
|---|---|---|---|
| `Concept` | X là gì? | Atomic, ontological | L1, L2 |
| `Insight` | Điều gì đúng/nên làm? | Atomic, epistemic | L2, L5 |
| `Method` | Làm X thế nào? | Atomic, procedural | L3 |
| `Example` | X trông thế nào thực tế? | Atomic, empirical | L3 |
| `Comparison` | A vs B theo tiêu chí Z? | Compound, analytical | L4 |
| `Synthesis` | A+B+C cho thấy gì mới? | Compound, integrative | L5, L6 |

---

## 5. Workflow làm việc trong wiki

### Ingest file nguồn

Khi user yêu cầu ingest/classify/add file vào wiki:

```text
[0] Copy file vào _sources/ → log vào _ingest-log.md
[1] Đọc source → validate
[2] Extract units → classify theo type
[3] Propose notes → chờ user confirm
[4] Draft notes → ghi vào đúng thư mục
[5] Cập nhật _index.md
```

Workflow chi tiết: theo các bước trên. Schema từng type xem tại `config/types/`.

### Query / tra cứu

Khi user hỏi hoặc tra cứu:
- Tìm theo tên note và keywords trước, domain chỉ là fallback.
- Nếu không tìm được → nói "không tìm thấy trong wiki", không suy diễn.

### Lint / health-check

Kiểm tra note theo schema từng type trong `config/types/`; báo cáo vi phạm rồi chờ confirm.

---

## 6. `_index.md` — Mục lục Wiki

Section **Compound Notes** nằm đầu file, flat, không phân domain.
Sections domain phía sau list concepts + insights.

Cập nhật `_index.md` khi tạo:
- concept mới,
- insight mới,
- compound note mới.

---

## 7. Conventions

### File naming
- Tất cả: kebab-case.
- `insights/`: tên = nội dung phát biểu.
- `concepts/`: tên = tên khái niệm.
- `methods/`: tên = tên quy trình/pattern.
- `examples/`: tên = mô tả ngắn trường hợp.

### Domain tagging
- Mọi note có `domain:` trong frontmatter.
- Domain dùng kebab-case, ví dụ `software-engineering`, `learning-methods`.

### Links
- Internal: `[[wikilink]]`.
- External: `[text](url)`.
- Citation: IEEE inline `[N]` + References section cuối note.

---

## 8. Standalone readability

Mỗi wiki note phải đứng độc lập — người đọc không cần mở file khác mới hiểu được nội dung.

- Term kỹ thuật chưa có concept note → giải thích inline `(mô tả)` hoặc đánh dấu `[[term]]`.
- Khi term đã có concept note → dùng `[[wikilink]]` thay vì giải thích lại.

---

## 9. Terminology

Wiki dùng tiếng Việt làm ngôn ngữ chính.

Trước khi viết note, draft, hoặc trả lời query, chạy Terminology Pass:
1. Xác định domain/context.
2. Nhận diện thuật ngữ kỹ thuật chính.
3. Phân loại và chọn canonical display form.
4. Kiểm tra: giữ jargon dạng English-first khi dịch sang tiếng Việt gây hiểu lầm.

---

## 10. Phát hiện vi phạm

- Compound note chứa proposition chưa extract → đề xuất tách ra `insights/`.
- Example thiếu link → đề xuất bổ sung.
- Atomic note thiếu IEEE references → đề xuất bổ sung.

Luôn đề xuất, không tự ý thay đổi nếu chưa được xác nhận.

---

## 11. Source từ Test Bank

`D:\workspace\test-bank\` là hệ thống riêng cho câu hỏi ôn thi. Điểm giao với wiki:

- File `test-bank/<subject>/<chapter>/chapter-theory.md` chứa lý thuyết cốt lõi tổng hợp từ nhiều câu hỏi.
- Khi distill, file này được copy vào `wiki/_sources/<subject>-<chapter>-theory-v<N>.md`.
- Ingest flow bình thường từ đây — không có gì đặc biệt.

**Lưu ý:** Source từ test bank thường không có IEEE references trực tiếp. Dùng Case 2 fallback trong `config/skills/ingest/references/ieee-citation.md` khi ingest.

---

*File này là source of truth cho policy làm việc trong wiki/.*
*Tạo: 2026-06-20*
