# Operating Rules

Các rule dưới đây là workflow tái sử dụng cho mọi dự án:

| Rule | Khi dùng | Điều kiện dừng |
|---|---|---|
| `00-init-docs.md` | Khởi tạo/refresh docs nền từ source | Không xác định được source/service thật. |
| `01-create-plan.md` | Có expectation và cần lập kế hoạch | AC, scope, service hoặc project config còn thiếu. |
| `02-implement-task.md` | Task đã được phê duyệt | Thiếu Gate 1, service-key hoặc verification plan. |
| `03-create-testcase.md` | Cần testcase trước/đồng thời plan | Expectation không có AC đo được. |
| `04-create-impact.md` | Cần blast-radius trước/sau code | Không đủ evidence về vùng tác động. |

Rule 01 và 02 dùng project-config. Sao chép file `.project-config.template.md`, bỏ hậu tố `.template`, rồi điền theo dự án trước khi dùng.

Các rule luôn ưu tiên evidence thật, scope đã duyệt, bảo mật dữ liệu và quyền dừng khi thiếu context.
