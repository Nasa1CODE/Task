# Rule 00 — Initialize / Refresh Base Docs

## Mục tiêu

Khảo sát source thật theo service registry và tạo **diff đề xuất** cho `architecture.md`, `specs/`, `databases/README.md` và `design.md`. Đây là CONTENT; không copy từ dự án khác.

## Đầu vào

- `AGENTS.md`: registry và ranh giới service.
- `project.md`, convention service, source repo đúng scope.
- Config/source chỉ để đọc tên khóa và hành vi; không chép secret.

## Quy trình

1. Kiểm layout, trạng thái Git và đọc PROFILE.
2. Với mỗi service: xác định entry point, layer business/data, contract API/message, storage, auth và integration thực tế.
3. Nhóm module theo domain; mỗi domain một file trong `specs/`.
4. Ghi architecture với trách nhiệm service, luồng, dependency và ranh giới trust/data.
5. Ghi database theo object thật, ownership và quy tắc migration/rollback.
6. Chỉ khi có frontend, ghi design system từ component/style có thật.
7. Đưa mọi điểm chưa chắc vào `Câu hỏi mở` cùng owner.

## Điều cấm và checklist

- Không bịa integration, class, method, queue/topic hay schema.
- Mỗi flow phải nêu service-key và entry point có thật.
- Không ghi secret, token, PII hoặc dữ liệu production.
- Chỉ đề xuất diff; TechLead duyệt Gate 2 trước merge.
