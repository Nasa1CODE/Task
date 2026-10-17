# Rule 04 — Create / Finalize Impact

## Mục đích

`impact.md` là bản đồ blast radius cho BA, DEV, QA và TechLead: cái gì đổi, cái gì được xác nhận không đổi, rủi ro và việc chưa kiểm chứng.

## Quy trình

1. Tạo draft khi plan dựa vào expectation, architecture và source vùng tác động.
2. Cập nhật khi phát hiện service/file/contract/data object mới.
3. Finalize sau implementation dựa trên diff và verification thật.

## Bắt buộc có

- Scope logic/data/schema/contract/config.
- Affected components theo layer, file/object và mức ảnh hưởng có lý do.
- Confirmed-unaffected cụ thể, không dùng câu “mọi thứ còn lại”.
- Rủi ro và hạn chế: auth, public exposure, tenant/data leak, backward compatibility, migration/rollback khi phù hợp.
- Việc chưa kiểm chứng có lý do rõ ràng và kết luận/recommendation phát hành.
