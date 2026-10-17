# Agent Guide — <PROJECT_NAME>

## 1. Metadata

```yaml
project: <PROJECT_NAME>
owner: TBD — owner: <ROLE>
docs_repo: <REPOSITORY_URL>
ticket_prefix: <TICKET_PREFIX>
docs_branch: <DOCS_BRANCH>
language_for_ai_replies: Vietnamese
```

Mọi giá trị chưa kiểm chứng phải ghi `TBD — owner: <ROLE>`, không đoán.

## 2. Bắt đầu phiên làm việc

Trước khi plan, code, test hoặc review:

1. Chạy `scripts/check-layout.sh`; nếu remote có thể truy cập, chạy `git pull`.
2. Đọc `AGENTS.md`, `README.md`, `project.md`, `architecture.md`, index `specs/README.md` và spec domain liên quan.
3. Đọc rule phù hợp cùng project-config của rule đó.
4. Nếu có ticket, đọc toàn bộ `features/<TICKET-ID>/` trước khi tác động source.
5. Nếu thay UI, đọc thêm `design.md`; nếu thay data, đọc `databases/README.md` và tài liệu object liên quan.

Thiếu profile/config cần thiết thì dừng và hỏi; không tự suy đoán fact dự án.

## 3. Service registry

| service-key | Repo path | Role | Stack | Datastore |
|---|---|---|---|---|
| `<service-key>` | `../<repo-folder>` | `<role>` | `<stack>` | `<datastore>` |

Service đích phải được khai ở `plan.md`, cột Service trong `tasks.md`, hoặc yêu cầu người dùng. Không xác định service chỉ bằng tên thư mục/grep; một task nhiều repo phải nêu rõ từng repo.

## 4. Feature workflow và gate

Mỗi feature ở `features/<TICKET-ID>/` gồm `expect.md`, `plan.md`, `tasks.md`, `impact.md`, `testcase.md`, `report.md`; tạo `deploy.md` chỉ khi chạm DB, config, queue/topic hoặc hạ tầng.

```text
draft → plan-review → approved → in-progress → testing → done
                                      └→ cancelled
```

- `expect.md` là nguồn duy nhất của trạng thái feature.
- `plan.md` có `plan-status: draft | plan-review | approved`.
- Task dùng `pending | in-progress | completed | blocked | cancelled`; task cancelled phải có lý do.
- **Gate 1:** DEV self-review + TechLead approve có tên, ngày và link bằng chứng; chỉ sau đó mới code.
- **Gate 2:** mọi thay đổi architecture/spec/schema/design chỉ là diff đề xuất cho TechLead duyệt.

## 5. An toàn, chất lượng và scope

- Chỉ làm trong scope đã duyệt; giữ `tasks.md`, `impact.md`, `report.md` khớp thực tế.
- Không ghi secrets, token, password, connection string, dữ liệu khách hàng hay payload nhạy cảm vào docs/log.
- Mọi thay đổi data phải kiểm authorization/tenant boundary và backward compatibility của API/message contract.
- Không sửa generated code, build output, lockfile, runtime config, certificate/secret, hoặc legacy module nếu không được yêu cầu rõ.
- Sau khi code, chạy lệnh verification của service; không chạy được phải ghi `N/A` cùng lý do kiểm chứng được.

## 6. Git và release

1. Trước thay đổi: kiểm branch, `git status`, diff và trạng thái các repo trong scope.
2. Không commit/push/merge/deploy khi chưa được ủy quyền rõ.
3. Commit/branch/PR phải theo `project.md`; report ghi commit/PR thật.
4. Dừng trước khi commit nếu diff ngoài scope hoặc chứa runtime config/secret.

## 7. Nguồn sự thật

| Câu hỏi | Đọc trước |
|---|---|
| Kiến trúc/tích hợp | `architecture.md` |
| Nghiệp vụ/permission/state | `specs/` |
| UI | `design.md` |
| Data/schema | `databases/` |
| Quy ước code | `conventions/` |
| Ticket | `features/<TICKET-ID>/` |
