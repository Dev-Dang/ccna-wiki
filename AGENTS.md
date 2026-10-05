---
_organized: true
---
# AGENTS.md — Wiki Agent Instructions

> Hướng dẫn bổ sung dành cho AI agent làm việc trong `wiki/`.
> Đọc `CLAUDE.md` trước, sau đó đọc file này.

---

## Quy tắc cơ bản

1. **Không tự ý tạo hoặc sửa note** khi chưa có confirm từ user.
2. **Proposal trước, action sau** — mọi thay đổi đều phải được trình bày rõ và chờ xác nhận.
3. **Đọc type schema** tại `config/types/<type>.md` trước khi draft bất kỳ note nào.
4. **Cập nhật `_index.md`** sau mỗi note được tạo.
5. **Không sửa `_sources/`** — chỉ copy vào, không xoá, không ghi đè.

---

## Khi ingest

Áp dụng workflow ingest trong `CLAUDE.md` §5; schema từng type tại `config/types/`.

Thứ tự bắt buộc:
```
Copy → Log → Read → Validate → Extract → Classify → Propose → Confirm → Draft → Write → Index
```

Không skip bước Propose/Confirm.

---

## Khi query

- Ưu tiên search theo tên note và keywords trong `_index.md`.
- Nếu không thấy → nói rõ "không tìm thấy trong wiki".
- Không hallucinate nội dung không có trong wiki.

---

## Khi lint

Kiểm tra note theo schema trong `config/types/`; báo cáo vi phạm và propose fix — không tự sửa.
