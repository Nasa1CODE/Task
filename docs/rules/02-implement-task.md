# Rule 02 — Implement Approved Task

## Điều kiện bắt buộc

- `expect.md` đang `approved`.
- `plan.md` có bằng chứng Gate 1 đầy đủ.
- Task có service-key hợp lệ và `02-implement-task.project-config.md` đã được điền.
- Branch/working tree của mọi repo trong scope đã được kiểm tra.

## Quy trình

1. Đọc convention đúng service và chỉ sửa repo/path được duyệt.
2. Thực hiện từng task; cập nhật `pending → in-progress → completed` bằng sự kiện thật.
3. Bảo toàn authorization/data boundary và tương thích API/message/DB.
4. Chạy build/test/manual verification theo plan; giữ lệnh và output kết quả thật.
5. Finalize `impact.md`, tạo `report.md`; ghi file thay đổi, sai khác, hạn chế, verification và commit/PR thật.
6. Tạo `deploy.md` nếu có DB/config/topic/queue/hạ tầng, gồm thứ tự rollout, backup, smoke check và rollback.
7. Đề xuất diff docs nền cho Gate 2.

## Điều cấm

- Không bịa test result; không làm ngoài plan; không tự đổi status feature sang done.
- Không log/chép secrets, token, credential, PII hoặc payload nhạy cảm.
- Không tự commit, push, merge hay deploy nếu không được ủy quyền.
