# Team

## Thông tin nhóm

- Tên nhóm: Something
- Lớp: L3A
- Repo nhóm: `K5-L3A-DAY08-Something-FromWorkflowsToAgents` (xem [SUBMISSION.md](SUBMISSION.md))
- URL repo, nhánh nộp, commit chốt:

## Thành viên

| STT | Họ tên | MSSV | Email | GitHub | Vai trò |
| --: | ------ | ---- | ----- | ------ | ------- |
|   1 | Vũ Trọng Quang | 2A202603535 | trongquangvu2004@gmail.com | Quangvu256 | Single agent (shape B) |
|   2 | Nguyễn Hoàng Cương | 2A202603568 | olivercuongnguyen@gmail.com | olivercuongnguyen-ca | Orchestrator (shape C), benchmark |

Gợi ý chia vai:

- Nhóm 6 người: integration; workflow; single agent; orchestrator; ledger/policy/crash; eval/report.
- Nhóm 4 người: workflow + integration; single; orchestrator + handoff; ledger + evaluation.

## Bảng đóng góp

`python -m lab8.check_submission` đọc bảng này bằng máy: giữ nguyên tiêu đề `## Bảng đóng góp` và các cột. Mỗi người một dòng: commit có nội dung và ít nhất một test đã thêm hoặc sửa.

| Người | Vai | Commit | Test đã thêm hoặc sửa |
| --- | --- | --- | --- |
| Vũ Trọng Quang (2A202603535) | Single agent (shape B) | `0907a74` (PR #3) | `src/tests/test_single_shape.py` |
| Nguyễn Hoàng Cương (2A202603568) | Orchestrator (shape C), benchmark | `dd07988`, `b8fc761` | `src/tests/test_2A202603568_orchestrator.py`: `test_ticket_delay_never_changes_the_money`, `test_handoff_is_read_only_and_trace_has_no_secret`, `test_full_quote_is_passed_so_host_pays_only_the_difference`, `test_zero_quote_does_not_call_the_host`, `test_host_rejection_keeps_full_quote_and_audit` |

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
