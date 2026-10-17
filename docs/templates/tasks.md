# Tasks — <TICKET-ID>

## Tổng quan

| # | Task | Service | Status | Commit/PR |
|---|---|---|---|---|
| 1 | | | pending | |

## Task 1 — <một việc cụ thể>

- **Status:** pending
- **Service:** `<service-key>`
- **Mô tả:**
- **Tiêu chí chấp nhận:**
  - <liên kết AC>
- **Kiểm chứng:** `<lệnh/bước>` → <kết quả kỳ vọng>
- **Phụ thuộc:**
- **File dự kiến chạm:**
- **Commit/PR:**

## Quy tắc tách task

- Tách khi task chạm nhiều service, hơn một vùng độc lập hoặc có hơn một outcome.
- Một task chỉ có một service-key; việc đa repo phải có task riêng theo repo.
- `blocked` ghi blocker/owner; `cancelled` ghi lý do.

Status hợp lệ: `pending`, `in-progress`, `completed`, `blocked`, `cancelled`. Task bị huỷ phải nêu lý do.
