# Team

## Thông tin nhóm

- Tên nhóm: Something
- Lớp: L3A
- Repo nhóm: `K5-L3A-DAY08-Something-FromWorkflowsToAgents`
- URL repo: https://github.com/olivercuongnguyen-ca/K5-L3A-DAY08-Something-FromWorkflowsToAgents
- Nhánh cá nhân: `giang`
- Commit cá nhân: `93c0930` (C8 – Add workflow unknown claim test)

## Thành viên

| STT | Họ tên | MSSV | Email | GitHub | Vai trò |
| --: | ------ | ---- | ----- | ------ | ------- |
| 1 | Vũ Trọng Quang | 2A202603535 | trongquangvu2004@gmail.com | Quangvu256 | Single agent (shape B) |
| 2 | Nguyễn Thị Hương Giang | 2A202603538 | 26ai.giangnth3@vinuni.edu.vn | nthgiang311 | Deterministic Workflow (shape A) |

## Bảng đóng góp

| Người | Vai | Commit | Test đã thêm hoặc sửa |
| --- | --- | --- | --- |
| Vũ Trọng Quang (2A202603535) | Single agent (shape B) | `0907a74` (PR #3) | `src/tests/test_single_shape.py` |
| Nguyễn Thị Hương Giang (2A202603538) | Deterministic Workflow (shape A) | `51db0c9`, `abcfc5b`, `93c0930` | `src/tests/test_202603404_workflow_unknown_claim.py` – `test_workflow_unknown_claim_does_not_create_refund` |

## Cá nhân

### 2A202603535_VuTrongQuang

Báo cáo chi tiết: [report/team/2A202603535_VuTrongQuang.md](report/team/2A202603535_VuTrongQuang.md).

### Phần việc phụ trách

- Shape B — Single ReAct agent: `src/lab8/shapes/single.py` (CP2, CP3).
- Kiểm chứng shape single trong cổng crash / approval / secret (CP4).

### Đóng góp cụ thể

- CP2: thay stub bằng `run_react` (`max_steps=8`). Agent chỉ dùng read tool, tiền do `compute_compensation` tính, trả về qua `finish()` có trace ở mọi nhánh. Thiếu `order_id` thì trả `needs_info`, không parse từ ticket. Truyền quote đầy đủ vào `durability.issue_refund` (T13, T14).
- CP3: truyền `crash_at` vào `issue_refund`, không bắt `InjectedCrash`. Gọi lại hoặc resume đều dựa vào ledger idempotent `refund:{claim_id}` (T11, T12).
- CP4: trace không ghi ticket, `parent_secret`, `raw_headers` hay order row thô.

### Bằng chứng

- Commit `0907a74`, merge qua PR #3 (`13013af`).
- `python -m pytest -q -m student -o addopts=` → `248 passed`; `python -m pytest -q` → xanh.
- `reports/benchmark.json`, shape `single`: pass_rate 1.0, pass^5 1.0, variance 0.0, cost/success 38.73 VND (mô phỏng).
- Run id: `R-single-C-014-s11`, `R-single-C-004-s11`, `R-single-C-005-s11`.

### 2A202603538_NguyenThiHuongGiang

Báo cáo chi tiết: [report/team/2A202603538_NguyenThiHuongGiang.md](report/team/2A202603538_NguyenThiHuongGiang.md).

### Phần việc phụ trách

- Shape A — Deterministic Workflow: `src/lab8/shapes/workflow.py` (CP2, CP3).
- Bổ sung kiểm thử hành vi workflow theo yêu cầu C8.
- Chạy kiểm thử và hidden benchmark self-check cho ba kiến trúc workflow, single và orchestrator.

### Đóng góp cụ thể

- CP2: triển khai `run_claim()` theo luồng xác định: đọc claim, kiểm tra thông tin, tra cứu đơn hàng, kiểm tra quyền truy cập, tính toán khoản bồi thường và thực hiện hoàn tiền. Xử lý các trạng thái `needs_info`, `escalated`, `denied`, `resolved` và `failed`. Sử dụng `compute_compensation()` để tính số tiền thay vì để LLM tự quyết định.
- CP3: truyền `crash_at` xuống `issue_refund()`, không bắt và nuốt ngoại lệ `InjectedCrash`, giữ nguyên cơ chế xử lý crash và retry của durability layer.
- C8: thêm test `test_workflow_unknown_claim_does_not_create_refund()` để xác nhận claim không tồn tại phải trả về `failed`, không tạo refund effect và không ghi nhận audit hoàn tiền.

### Bằng chứng

- Commit `51db0c9` – Implement deterministic workflow shape.
- Commit `abcfc5b` – Implement workflow crash recovery.
- Commit `93c0930` – Add workflow unknown claim test.
- Kiểm thử workflow: `62 passed, 240 deselected`.
- Kiểm thử riêng C8: `1 passed`.
- Kiểm thử toàn bộ test suite: `55 passed, 248 deselected`.
- Hidden benchmark self-check: 200 trials cho mỗi kiến trúc; workflow có mean steps 3.55, mean model calls 0, tổng simulated cost 7,050 VND; không ghi nhận error trials hoặc leak suspect trials. Dữ liệu không có nhãn nên không tính được pass rate chính thức.
