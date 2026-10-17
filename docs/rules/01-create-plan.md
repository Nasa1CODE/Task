# Rule 01 — Create Plan

## Điều kiện và input

- `features/<TICKET-ID>/expect.md` tồn tại.
- `01-create-plan.project-config.md` đã được điền từ template.
- Đọc architecture/spec/database/design/convention liên quan.

## Definition of Ready

Expectation phải có actor, input/output, state/behavior, acceptance criteria đo được, out-of-scope và câu hỏi mở. Nếu thiếu hoặc service chưa khai rõ: chỉ tạo draft nêu **blocking questions**, không tạo thiết kế chi tiết.

## Quy trình

1. Resolve service qua registry; không dùng suy đoán tên repo.
2. Đọc đủ source vùng tác động để xác nhận nguyên nhân, contract, data/permission và blast radius.
3. Tạo `plan.md`: mục tiêu trace về AC, cách làm từng bước, dependency, validation command, risk/rollback và docs update map.
4. Tạo `tasks.md`: task nhỏ, một service rõ ràng, status word-form, tiêu chí chấp nhận và verification cụ thể.
5. Tạo `impact.md` nháp: affected + confirmed-unaffected + risk + unverified.
6. Để trống block Gate 1; AI không đổi expectation sang approved và không viết code.

## Tự review trước Gate 1

- Mọi AC có task/test strategy; scope và out-of-scope rõ.
- Contract, security, permission, tenant/data boundary và migration/config/queue impact đã nêu.
- Lệnh verification có kết quả kỳ vọng, không ghi chung chung “run test”.
