# Báo cáo cá nhân — MSSV_HoTen

## 1. Thông tin thành viên

- Họ tên: Nguyễn Thị Hương Giang
- MSSV: 2A202603538
- Email: 26ai.giangnth3@vinuni.edu.vn
- GitHub: [https://github.com/nthgiang311](https://github.com/nthgiang311)

## 2. Phần việc phụ trách

Phụ trách triển khai **Shape A – Deterministic Workflow** trong Lab 8.

Các nhiệm vụ chính:
- Xây dựng luồng xử lý khiếu nại theo các bước xác định.
- Đọc thông tin claim và tra cứu order thông qua các tool được cung cấp.
- Tính toán khoản bồi thường dựa trên dữ liệu đơn hàng và chính sách.
- Tích hợp cơ chế thực hiện refund thông qua `durability.issue_refund`.
- Xử lý tham số `crash_at` theo yêu cầu CP3.

## 3. Đóng góp cụ thể

### Commit chính

| Commit | Nội dung |
|---|---|
| `51db0c9` | Implement deterministic workflow shape (CP2) |
| `abcfc5b` | Implement workflow crash recovery (CP3) |

### Đóng góp kỹ thuật

**CP2 – Deterministic Workflow**
- Triển khai hàm `run_claim()` theo signature được quy định.
- Sử dụng `get_claim_view()` để đọc thông tin khiếu nại.
- Sử dụng `lookup_order()` để kiểm tra quyền truy cập đơn hàng.
- Sử dụng `compute_compensation()` để tính khoản bồi thường.
- Phân loại các trường hợp `needs_info`, `escalated`, `denied`, `resolved` và `failed`.
- Sử dụng `finish()` để tạo kết quả và ghi trace.

**CP3 – Crash handling**
- Truyền `crash_at` vào `durability.issue_refund()`.
- Loại bỏ cơ chế bắt ngoại lệ `InjectedCrash` trong Workflow để ngoại lệ được truyền lên tầng gọi.
- Giữ nguyên logic xử lý nghiệp vụ và cơ chế hoàn tiền của CP2.

### Quyết định kỹ thuật
- Số tiền hoàn được tính bằng code, không giao quyền quyết định cho LLM.
- Sử dụng dữ liệu đơn hàng thay vì tin vào thông tin delay do khách hàng cung cấp trong ticket.
- Giao việc kiểm tra quyền, giới hạn hoàn tiền và ghi nhận giao dịch cho durability layer.
- Không ghi thông tin nhạy cảm của khách hàng vào trace.

## 4. Bằng chứng

- Repository: https://github.com/olivercuongnguyen-ca/K5-L3A-DAY08-Something-FromWorkflowsToAgents
- Branch: `giang`
- Commit CP2: `51db0c9`
- Commit CP3: `abcfc5b`
- File triển khai: `src/lab8/shapes/workflow.py`

### Kết quả kiểm thử

Lệnh kiểm thử:

`python -m pytest -q -m student -o addopts= -k workflow`

Kết quả sau CP2:

`62 passed, 240 deselected`

Kết quả sau khi chỉnh sửa CP3:

`62 passed, 240 deselected`

Chưa ghi nhận kết quả crash suite 20 tình huống của CP4.

## 5. Công cụ AI đã dùng

**ChatGPT**
- Hỗ trợ đọc và phân tích yêu cầu của Workflow trong Lab 8.
- Hỗ trợ xây dựng và chỉnh sửa logic triển khai Workflow.
- Hỗ trợ giải thích cơ chế xử lý ngoại lệ và crash recovery.

**Cách tự kiểm tra kết quả:**
- Đọc lại source code và đối chiếu với yêu cầu trong tài liệu Lab 8.
- Trực tiếp chạy pytest trên môi trường local.
- Kiểm tra kết quả 62/62 test Workflow đạt yêu cầu.

## 6. Tự đánh giá

Qua quá trình triển khai Workflow, tôi hiểu rõ hơn cách xây dựng một deterministic workflow trong hệ thống xử lý khiếu nại, đặc biệt là việc phân tách trách nhiệm giữa tầng nghiệp vụ và tầng durability.

Tôi đã thực hành:
- Thiết kế luồng xử lý theo các trạng thái nghiệp vụ.
- Sử dụng tool để truy xuất dữ liệu và tính toán kết quả.
- Kiểm soát quyền thực hiện refund thông qua host.
- Hiểu vai trò của idempotency và crash recovery.
- Sử dụng Git branch và commit riêng theo từng checkpoint.

Điểm cần tiếp tục hoàn thiện là kiểm thử sâu hơn các tình huống crash/recovery trong CP4 và kiểm tra tích hợp Workflow với các shape còn lại của nhóm.
