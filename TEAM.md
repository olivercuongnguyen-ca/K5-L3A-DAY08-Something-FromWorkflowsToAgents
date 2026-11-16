# Team

## Thông tin nhóm

- Tên nhóm:
- Lớp: L3A
- Repo nhóm: `K5-L3A-DAY08-TenNhom-FromWorkflowsToAgents` (xem [SUBMISSION.md](SUBMISSION.md))
- URL repo, nhánh nộp, commit chốt:

## Thành viên

| STT | Họ tên | MSSV | Email | GitHub | Vai trò |
| --: | ------ | ---- | ----- | ------ | ------- |
|   1 | Vũ Trọng Quang | 2A202603535 | trongquangvu2004@gmail.com | Quangvu256 | Single agent (shape B) |
|   2 |        |      |       |        |         |

Gợi ý chia vai:

- Nhóm 6 người: integration; workflow; single agent; orchestrator; ledger/policy/crash; eval/report.
- Nhóm 4 người: workflow + integration; single; orchestrator + handoff; ledger + evaluation.

## Bảng đóng góp

`python -m lab8.check_submission` đọc bảng này bằng máy: giữ nguyên tiêu đề `## Bảng đóng góp` và các cột. Mỗi người một dòng: commit có nội dung và ít nhất một test đã thêm hoặc sửa.

| Người | Vai | Commit | Test đã thêm hoặc sửa |
| --- | --- | --- | --- |
| Vũ Trọng Quang (2A202603535) | Single agent (shape B) | `0907a74` (PR #3) | `src/tests/test_2A202603535_single_shape.py`: `test_cp3_t11_retry_same_claim_is_one_refund`, `test_cp3_t12_crash_is_not_swallowed_and_resume_is_once`, `test_cp4_crash_point_never_creates_two_refunds`, `test_cp4_t07_expired_approval_no_refund`, `test_cp4_t13_fully_refunded_order_no_refund`, `test_cp4_no_secret_or_raw_row_in_trace_or_stdout`, `test_cp2_*` (trace, needs_info, cap T14) |

## Cá nhân

Mỗi thành viên đồng thời tạo một báo cáo riêng trong `report/` theo dạng:

```text
report/team/MSSV_HoTen.md
```

Ví dụ: `report/team/20241234_NguyenVanAn.md`.

Sao chép mục dưới đây cho từng thành viên.

## 2A202603535_VuTrongQuang

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

## MSSV_HoTen

### Phần việc phụ trách

-

### Đóng góp cụ thể

-

### Bằng chứng

-
