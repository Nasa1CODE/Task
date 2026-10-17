# Project Documentation Kit

Khung **spec-driven development** để BA, DEV, QA, TechLead và AI làm việc với cùng một nguồn sự thật. Repo này quản lý yêu cầu, kiến trúc, tác động, kế hoạch, bằng chứng kiểm thử và triển khai; source code vẫn thuộc các repo/service của dự án.

## Ba lớp tài liệu

| Lớp | Nội dung | Khi dùng cho dự án mới |
|---|---|---|
| **ENGINE** | `rules/`, `templates/`, `commands/`, `scripts/` | Giữ nguyên; chỉ thay đổi khi quy trình chung thay đổi. |
| **PROFILE** | `AGENTS.md`, `project.md`, `conventions/`, `*.project-config.md` | Điền lại từ cách vận hành và source thật của dự án. |
| **CONTENT** | `architecture.md`, `specs/`, `databases/`, `design.md`, `features/` | Sinh mới; tuyệt đối không sao chép fact, schema hoặc ticket dự án cũ. |

## Khởi tạo dự án mới

1. Điền metadata, service registry và chính sách an toàn trong `AGENTS.md`.
2. Điền stack, Git/release, data/security policy ở `project.md`.
3. Sao chép hai `rules/*.project-config.template.md` thành file config tương ứng và điền giá trị dự án.
4. Tạo convention cho mỗi nhóm service từ `conventions/_template.md`.
5. Chạy Rule 00 để khảo sát source và **đề xuất** `architecture.md`, `specs/`, `databases/`, `design.md`.

## Vòng đời feature

```text
expect → plan + tasks + impact + testcase → Gate 1 → implementation → report/deploy → Gate 2
```

- `expect.md` do BA sở hữu và là nguồn duy nhất của trạng thái feature.
- **Gate 1:** DEV review và TechLead phê duyệt kế hoạch trước khi code.
- **Gate 2:** TechLead phê duyệt mọi cập nhật tài liệu nền sau khi có bằng chứng thực tế.
- `report.md` chỉ được ghi lệnh, kết quả và file thay đổi thật.

## Bộ rule

| Rule | Mục đích |
|---|---|
| `00-init-docs` | Khảo sát source và xây dựng tài liệu nền. |
| `01-create-plan` | Tạo kế hoạch, task và đánh giá tác động từ expectation. |
| `02-implement-task` | Thực thi task đã duyệt, xác minh và báo cáo. |
| `03-create-testcase` | Sinh testcase từ acceptance criteria. |
| `04-create-impact` | Dựng/finalize đánh giá blast radius. |

AI không tự phê duyệt, commit, push, merge hoặc deploy nếu chưa được người dùng/owner ủy quyền rõ ràng.
