# Commands

Các command là lớp tiện ích gọi Rule 00–04. Sao chép vào công cụ agent đang dùng hoặc gọi rule tương ứng trực tiếp. Không chứa đường dẫn máy tính, tên dự án hay ticket prefix cụ thể.

| Command | Input | Output |
|---|---|---|
| `init-docs` | service registry + source | diff architecture/specs/database/design |
| `create-plan <TICKET-ID>` | expectation + base docs | plan, tasks, impact draft |
| `create-testcase <TICKET-ID>` | expectation | testcase trace AC |
| `create-impact <TICKET-ID>` | expectation + plan + source | impact draft/final |
| `implement-task <TICKET-ID>` | approved plan/task | code evidence, report, deploy when needed |

Mỗi command phải đọc rule tương ứng và project-config khi rule yêu cầu. Thiếu context hoặc approval là điều kiện dừng, không phải lý do để đoán.
