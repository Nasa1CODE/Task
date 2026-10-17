# Rule 03 — Create Testcase

## Nguồn và nguyên tắc

Sinh `features/<TICKET-ID>/testcase.md` từ `expect.md`, không suy ra hành vi từ code/plan. Plan chỉ dùng để biết service và cách chạy verification.

## Yêu cầu

1. Có bảng traceability AC → testcase; mỗi AC ít nhất một case.
2. Bao phủ luồng chính, input biên, lỗi/negative, hồi quy vùng confirmed-unaffected, permission và tenant/data isolation khi phù hợp.
3. Case phải có tiền đề, dữ liệu an toàn, bước tái lập được và kết quả quan sát được.
4. Tích hợp không chạy local phải là `manual`, nêu lý do và người nghiệm thu.
5. Không dùng customer data, secret hoặc token thật.
